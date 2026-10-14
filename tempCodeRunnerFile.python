import streamlit as st
import pandas as pd

st.set_page_config(page_title="📚 BookFindr - Smart Book Explorer", layout="centered")

df = pd.read_csv("books.csv")

st.sidebar.header("🎯 Find Your Next Favorite Book")

with st.sidebar.form("filters"):
    search_query = st.text_input("🔎 Search by Title")
    price_limit = st.slider("💰 Max Price (₹)", 0, int(df["Price"].max()), 30)
    rating = st.selectbox("⭐ Minimum Rating", [0, 1, 2, 3, 4, 5], index=3)

    sort_by = st.selectbox(
        "📊 Sort By",
        [
            "Price Low to High",
            "Price High to Low",
            "Rating High to Low"
        ]
    )

    submitted = st.form_submit_button("🔍 Search Books")

st.markdown(
    "<h1 style='text-align:center;'>📖 Welcome to BookFindr</h1>",
    unsafe_allow_html=True
)

st.markdown(
    "<p style='text-align:center;'>Find books based on title, price and ratings 📚</p>",
    unsafe_allow_html=True
)

st.markdown("---")

if submitted:

    filtered = df[
        (df["Price"] <= price_limit) &
        (df["Rating"] >= rating)
    ]

    if search_query:
        filtered = filtered[
            filtered["Title"].str.contains(
                search_query,
                case=False,
                na=False
            )
        ]

    if sort_by == "Price Low to High":
        filtered = filtered.sort_values("Price")

    elif sort_by == "Price High to Low":
        filtered = filtered.sort_values(
            "Price",
            ascending=False
        )

    elif sort_by == "Rating High to Low":
        filtered = filtered.sort_values(
            "Rating",
            ascending=False
        )

    st.markdown(
        f"### 📚 Books matching your search"
    )

    if filtered.empty:
        st.warning("🙁 No books match your filters.")
    else:

        st.success(f"Found {len(filtered)} matching books")

        col1, col2, col3 = st.columns(3)

        with col1:
            st.metric("Books Found", len(filtered))

        with col2:
            st.metric(
                "Average Price",
                f"₹{round(filtered['Price'].mean(),2)}"
            )

        with col3:
            st.metric(
                "Highest Rating",
                int(filtered["Rating"].max())
            )

        st.download_button(
            "⬇️ Download Filtered Books",
            filtered.to_csv(index=False),
            "filtered_books.csv",
            "text/csv"
        )

        st.subheader("📈 Price Distribution")
        st.bar_chart(filtered["Price"])

        for _, row in filtered.iterrows():

            stars = "⭐" * int(row["Rating"])

            st.markdown(
                f"""
                <div style="
                    display:flex;
                    gap:20px;
                    align-items:center;
                    padding:15px;
                    margin-bottom:15px;
                    border:1px solid #ddd;
                    border-radius:10px;
                    background-color:#f9f9f9;
                ">
                    <img src="{row['Image_URL']}" width="100">
                    <div>
                        <h4>{row['Title']}</h4>
                        <p>
                            💸 <b>Price:</b> ₹{row['Price']}
                            &nbsp;&nbsp;
                            ⭐ <b>Rating:</b> {stars}
                        </p>
                        <p>
                            <i>{row['Availability']}</i>
                        </p>
                    </div>
                </div>
                """,
                unsafe_allow_html=True
            )

else:
    st.info(
        "👈 Use the sidebar to search books by title, price, and rating."
    )