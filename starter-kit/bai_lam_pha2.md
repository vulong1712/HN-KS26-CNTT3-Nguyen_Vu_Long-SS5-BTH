## 4.2. Trích xuất tin nhắn đặt vé ra JSON

**KET QUA AI**

```text
[
  {
    "ma_goi": "SVT",
    "ngay_su_kien": "2026-12-20",
    "nguoi_lon": 2,
    "tre_em": 1,
    "loai_ve": "thuong",
    "ghi_chu": null
  },
  {
    "ma_goi": "FZ",
    "ngay_su_kien": "2027-01-05",
    "nguoi_lon": 3,
    "tre_em": 0,
    "loai_ve": null,
    "ghi_chu": "cần hỗ trợ xe lăn"
  },
  {
    "ma_goi": "VIP",
    "ngay_su_kien": null,
    "nguoi_lon": 4,
    "tre_em": 0,
    "loai_ve": "vip",
    "ghi_chu": "một người khiếm thị cần hỗ trợ tìm chỗ"
  }
]
```

**Link trò chuyện:** [Link Gemini](https://share.gemini.google/mnwDdcS2zS10)

## 4.3. Báo giá vé bằng Chain-of-Thought

**KET QUA AI**

```text

Suy luận từng bước:Tiền người lớn: Nhóm mua gói SVT (1.200.000 đồng/người lớn) cho 5 người lớn.
Phép tính: $5 \times 1.200.000 = 6.000.000$ đồng.Tiền trẻ em: Có 2 trẻ em (8 tuổi và 10 tuổi) nằm trong độ tuổi 5–11 tuổi (tính 70% giá người lớn = $70\% \times 1.200.000 = 840.000$ đồng/trẻ) và 1 bé 3 tuổi (dưới 5 tuổi nên miễn phí = 0 đồng).
Phép tính: $(2 \times 1.200.000 \times 70\%) + (1 \times 0) = 1.680.000$ đồng.Tiền vé: Tổng tiền vé người lớn và trẻ em.
Phép tính: $6.000.000 + 1.680.000 = 7.680.000$ đồng.Giảm giá nhóm: Nhóm có đúng 5 người lớn (đạt điều kiện từ 5 người lớn trở lên) nên được giảm 5% trên tiền vé.
Phép tính: $7.680.000 \times 5\% = 384.000$ đồng (Tiền vé sau giảm: $7.680.000 - 384.000 = 7.296.000$ đồng).Phụ thu: Có 1 người lớn chọn khu ghế ưu tiên (300.000 đồng/người).
Phép tính: $1 \times 300.000 = 300.000$ đồng.Tổng tiền: Tiền vé sau giảm cộng với phụ thu.
Phép tính: $7.296.000 + 300.000 = 7.596.000$ đồng.

[RESULT]: 7.596.000 đồng
```

**Link trò chuyện:** [Link Gemini](https://share.gemini.google/EUgRdbRb4pF0)

### b) Kiểm tra kết quả AI

Kết luận: AI tính ĐÚNG hoàn toàn các bước và kết quả cuối cùng (7.596.000 đồng).

## 4.4. Dây chuyền xử lý khiếu nại – kết quả Trạm 1

**KET QUA AI**

```text
{
  "loai_su_co": "sai_khu_vuc",
  "so_khach": 2,
  "so_hiep_bi_anh_huong": 2,
  "tom_tat": "Khách mua vé có mái che nhưng bị xếp sang khu không mái trong 2 hiệp đầu khiến ghế ướt do mưa."
}
```

**Link trò chuyện:** [Link Gemini](https://share.gemini.google/kZRgann6prL8)
