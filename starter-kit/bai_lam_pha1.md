## 4.1. Bảo vệ dữ liệu và chống ảo giác

### a) Ghi chú đã ẩn danh hoá

**GHI CHU DA AN DANH**

```text
Khán giả [TEN_KHACH_HANG], số điện thoại [SO_DIEN_THOAI], số CCCD [SO_CAN_CUOC_NHAN_DAN], email [DIA_CHI_EMAIL], đặt 3 vé gói SVT trận bán kết Cúp Sao Vàng ngày 18/12/2026 cho 2 người lớn và 1 trẻ em 9 tuổi. Khán giả đã chuyển khoản đặt cọc 1.500.000 đồng. Khán giả yêu cầu 3 ghế ngồi cạnh nhau gần lối đi.
```

### b) Hai câu lệnh chống ảo giác

**CAU LENH**

```text
Câu 1: Dựa trên file chính sách hoàn/huỷ vé mà tôi gửi hãy trả lời câu hỏi trên của tôi . Nếu các thông tin không có trong chính sách thì không tự bịa chính sách mà trả lời trực tiếp là "Chính sách về điều khoản này không tồn tại"
Câu 2: Khi bạn trả lời câu hỏi của tôi thì bạn hãy ghi rõ câu trả lời lấy từ mục hoặc điều khoản nào trong file chính sách hoàn huỷ đó

```

## 4.2. Trích xuất tin nhắn đặt vé ra JSON

**PROMPT**

```text
[Context]
Bạn là một chuyên gia phân tích tin nhắn khách hàng ,bạn có nhiệm vụ chuyển các tin nhắn thành định dạng JSON

[Request]Bạn hãy chuyển TN1,TN2,TN3 trong thẻ <tin_nhan> thành một mảng JSON 3 đối tượng,đúng thứ tự ,đúng 6 trừng như sau "ma_goi", "ngay_su_kien", "nguoi_lon", "tre_em", "loai_ve","ghi_chu",như tin nhắn mẫu

[Explanation] Khi chuyển thành JSON bạn cần bám theo quy tắc chuẩn hoá trong thẻ <quy_chuan>.Bạn có thể xem qua thử 2 ví dụ mẫu mà tôi đã thực hiển ở trong thẻ <vi_du>

<quy_chuan> 

    BẢNG MÃ GÓI    
    SVT – Sân Vận Động: vé trận đấu
    FZ – Fan Zone: khu cổ vũ
    VIP – Khán đài VIP

    QUY TẮC CHUẨN HOÁ (5 quy tắc)
    1. ma_goi lấy theo bảng mã gói.
    2. ngay_su_kien dạng YYYY-MM-DD; tin nhắn không ghi năm thì lấy năm 2026; không có ngày cụ thể thì null.
    3. Khách không nhắc trẻ em thì tre_em là 0 và mọi khách là người lớn.
    4. loai_ve chỉ nhận "thuong", "vip" hoặc null.
    5. ghi_chu chỉ ghi yêu cầu đặc biệt về chỗ ngồi, hỗ trợ di chuyển hoặc ăn uống; không có thì null.
    Câu trả lời không chào hỏi,không giải thích chỉ có dữ liệu JSON trả về


</quy_chuan>

<vi_du>

    Tin nhắn mẫu 1:
    Đặt giúp mình 2 vé VIP trận chung kết ngày 15/11/2026, 2 người lớn và 1 bé. Cần chỗ ngồi cạnh lối đi.
    Kết quả đúng:
    {"ma_goi": "VIP", "ngay_su_kien": "2026-11-15", "nguoi_lon": 2, "tre_em": 1, "loai_ve": "vip", "ghi_chu": "chỗ ngồi cạnh lối đi"}

    Tin nhắn mẫu 2:
    Cho mình một vé Fan Zone, ngày nào cũng được, mình đi một mình.
    Kết quả đúng:
    {"ma_goi": "FZ", "ngay_su_kien": null, "nguoi_lon": 1, "tre_em": 0, "loai_ve": "thuong", "ghi_chu": null}

</vi_du>

[Action] Đọc dữ liệu của TN khách hàng mà tôi đưa => Chuẩn hoá về đúng các trường dữ liệu => Trả đúng về định dang JSON

[Tone] Chuyên nghiệp,ngắn gọn,xúc tích

[Extras]Định dạng đầu ra chỉ là dữ liệu JSON

<tin_nhan>
    TN1: Mình muốn đặt gói Sân Vận Động trận tứ kết ngày 20/12 nha, 2 người lớn với 1 bé 8 tuổi, vé thường.

    TN2: Cho 3 người đi Fan Zone trận ngày 05/01/2027 ạ. Một người cần hỗ trợ xe lăn.

    TN3: Trận chung kết Cúp Sao Vàng tháng 12 còn vé không ạ? Nhà em 4 người lớn, có một người khiếm thị cần hỗ trợ tìm chỗ, lấy vé VIP.
</tin_nhan>


```

## 4.3. Báo giá vé bằng Chain-of-Thought

### a) Prompt

**PROMPT**

```text
Hãy đóng vai trò bạn là người tính tổng tiền cho yêu cầu báo giá .Bạn hãy đưa bảng giá, quy tắc và yêu cầu báo giá vào cặp dấu phân tách; yêu cầu suy luận từng bước theo thứ tự tiền người lớn → tiền trẻ em → tiền vé → giảm giá nhóm → phụ thu → tổng tiền, mỗi bước ghi phép tính; dòng cuối có dạng [RESULT: <tổng tiền> đồng].
<bang_gia_ve>
    BẢNG GIÁ VÉ (đồng/người lớn)
    SVT – Sân Vận Động: 1.200.000
    FZ – Fan Zone: 800.000
    VIP – Khán đài VIP: 2.500.000
</bang_gia_ve>

<quy_tac_tinh_gia>
    QUY TẮC TÍNH GIÁ
    1. Trẻ em từ 5 đến 11 tuổi tính 70% giá người lớn. Trẻ dưới 5 tuổi miễn phí.
    2. Nhóm từ 5 người lớn trở lên được giảm 5% trên tiền vé (tiền người lớn + tiền trẻ em).
    3. Phụ thu khu ghế ưu tiên: 300.000 đồng/người. Phụ thu không được giảm giá.
    4. Tổng tiền = tiền vé sau giảm + phụ thu.
</quy_tac_tinh_gia>

<yeu_cau_bao_gia>
    YÊU CẦU BÁO GIÁ
    Nhóm cổ động viên đi xem trận bán kết, mua gói SVT, gồm 5 người lớn, 2 trẻ em (8 tuổi và 10 tuổi) và 1 bé 3 tuổi. Có 1 người lớn chọn khu ghế ưu tiên.
</yeu_cau_bao_gia>
Hãy tính theo dữ liệu từ file JSON tôi cung cấp.

```json
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
"ghi_chu": "hỗ trợ xe lăn"
},
{
"ma_goi": "VIP",
"ngay_su_kien": null,
"nguoi_lon": 4,
"tre_em": 0,
"loai_ve": "vip",
"ghi_chu": "người khiếm thị cần hỗ trợ tìm chỗ"
}
]

```

## 4.4. Dây chuyền xử lý khiếu nại (Prompt Chaining)

### a) Prompt Trạm 1

**PROMPT**

```text
Hãy đóng vai làm một nhân viên chăm sóc khách hàng. Nhiệm vụ của bạn là đọc email khiếu nại trong thẻ <email> và chỉ trả về một đối tượng JSON đúng 4 trường theo quy định: {"loai_su_co": một trong "tran_gio" | "sai_khu_vuc" | "nhan_vien" | "khac", "so_khach": số nguyên, "so_hiep_bi_anh_huong": số nguyên hoặc null, "tom_tat": "không quá 20 từ"}

<email>

Tôi là khán giả mua vé gói SVT trận Cúp Sao Vàng ngày 02/11/2026, đi 2 người lớn. Ban tổ chức cam kết khu khán đài có mái che nhưng trong 2 hiệp đầu chúng tôi bị xếp sang khu không mái, trời mưa và ghế ướt. Nhân viên soát vé rất nhiệt tình, tôi không có gì phàn nàn về nhân viên. Tôi đề nghị ban tổ chức giải quyết thỏa đáng.

</email>
-

```

### b) Hai điều cần kiểm tra ở JSON đầu ra của Trạm 1

1. Đúng định dạng JSON
2. Các thông tin/các trường không được nằm ngoài chính sách

### c) Câu lệnh viết tiếp

**CAU LENH**

```text
Bạn vừa dừng ở chỗ "…Ban tổ chức Cúp Sao Vàng xin hoàn lại Quý khán giả khoản". Hãy viết tiếp câu trả lời từ đó"
```
