# Tính Toán Cốt Thép Theo TCVN bằng Python

Đây là một công cụ nhỏ viết bằng Python giúp:
- Tính diện tích cốt thép (As) cho cốt thép đơn.
- Gợi ý 3 phương án bố trí cốt thép (số lượng và đường kính thanh) phù hợp.


## Input

Người dùng sẽ nhập:
- Mômen uốn M (đơn vị: kNm)
- Kích thước dầm: b (mm), h (mm)
- Cấp độ bền bê tông (VD: B20, B25, ...)
- Loại cốt thép (VD: CB300-V, CB400-V, ...)


## Output

- Diện tích cốt thép yêu cầu As (mm²)
- Gợi ý 3 phương án bố trí thép (số lượng và đường kính)


## Ví dụ
==> Nhập các thông số thiết kế:
Mômen uốn M (kNm): 45
Chiều rộng dầm b (mm): 300
Chiều cao dầm h (mm): 500
Cấp độ bền bê tông (vd B20): B25
Loại thép (vd CB300-V): CB300-V

Rb = 14.5 MPa | Rs = 260 MPa | Rsc = 260 MPa
Diện tích cốt thép yêu cầu As = 859.7 mm²

Gợi ý 3 phương án chọn thép:
1. 3Φ20 → 942.6 mm²
2. 4Φ18 → 1018.0 mm²
3. 2Φ25 → 981.8 mm²









