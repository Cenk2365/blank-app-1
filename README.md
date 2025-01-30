def hesapla_karbon(ulasim, yemek, enerji, giyim, alisveris):
    karbon_verileri = {
        "Araba": 4.6,
        "Otobüs": 1.8,
        "Bisiklet": 0.2,
        "Yürüyüş": 0,
        "Et Yemeği": 5.4,
        "Vejetaryen Yemeği": 2.2,
        "Vegan Yemeği": 1.5,
        "Fosil Yakıtlı Elektrik": 3.2,
        "Yenilenebilir Elektrik": 0.5,
        "Hızlı Moda": 3.0,
        "Sürdürülebilir Moda": 1.0,
        "Az Alışveriş": 0.5,
        "Çok Alışveriş": 3.5
    }
    
    toplam_karbon = (karbon_verileri[ulasim] + karbon_verileri[yemek] + karbon_verileri[enerji] +
                     karbon_verileri[giyim] + karbon_verileri[alisveris])
    return toplam_karbon

def main():
    st.title("Karbon Ayak İzi Hesaplayıcı")
    st.subheader("Günlük Seçimlerinizi Yapın ve Karbon Ayak İzinizi Öğrenin!")
    
    ulasim = st.selectbox("Bugün nasıl ulaştınız?", ["Araba", "Otobüs", "Bisiklet", "Yürüyüş"])
    yemek = st.selectbox("Bugün ne yediniz?", ["Et Yemeği", "Vejetaryen Yemeği", "Vegan Yemeği"])
    enerji = st.selectbox("Elektrik kaynağınız nedir?", ["Fosil Yakıtlı Elektrik", "Yenilenebilir Elektrik"])
    giyim = st.selectbox("Giyim tercihiniz nedir?", ["Hızlı Moda", "Sürdürülebilir Moda"])
    alisveris = st.selectbox("Alışveriş sıklığınız nedir?", ["Az Alışveriş", "Çok Alışveriş"])
    
    if st.button("Hesapla"):
        toplam_karbon = hesapla_karbon(ulasim, yemek, enerji, giyim, alisveris)
        st.success(f"Toplam Günlük Karbon Ayak İziniz: {toplam_karbon:.2f} kg CO₂")
        
        # Yapay Zeka Önerisi
        if toplam_karbon > 7:
            st.warning("Karbon ayak izinizi azaltmak için bisiklet veya yürüyüşü tercih edebilir, sürdürülebilir modaya yönelebilirsiniz!")
        else:
            st.info("Tebrikler! Düşük karbon ayak izine sahipsiniz. Daha sürdürülebilir seçimler yapmaya devam edin!")
        
        # Grafik Gösterimi
        fig, ax = plt.subplots()
        kategoriler = ["Ulaşım", "Yemek", "Enerji", "Giyim", "Alışveriş"]
        degerler = [hesapla_karbon(ulasim, yemek, enerji, giyim, alisveris) - hesapla_karbon("Yürüyüş", yemek, enerji, giyim, alisveris),
                    hesapla_karbon(ulasim, yemek, enerji, giyim, alisveris) - hesapla_karbon(ulasim, "Vegan Yemeği", enerji, giyim, alisveris),
                    hesapla_karbon(ulasim, yemek, enerji, giyim, alisveris) - hesapla_karbon(ulasim, yemek, "Yenilenebilir Elektrik", giyim, alisveris),
                    hesapla_karbon(ulasim, yemek, enerji, giyim, alisveris) - hesapla_karbon(ulasim, yemek, enerji, "Sürdürülebilir Moda", alisveris),
                    hesapla_karbon(ulasim, yemek, enerji, giyim, alisveris) - hesapla_karbon(ulasim, yemek, enerji, giyim, "Az Alışveriş")]
        
        ax.bar(kategoriler, degerler, color=['red', 'orange', 'blue', 'green', 'purple'])
        ax.set_ylabel("Karbon Salınımı (kg CO₂)")
        ax.set_title("Seçimlerinizin Karbon Ayak İzi")
        st.pyplot(fig)

if __name__ == "__main__":
    main()

