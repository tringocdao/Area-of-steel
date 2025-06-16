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


## Cách chạy chương trình

1. Cài đặt Python 3.13
2. Chạy file bằng lệnh:
   [Upimport math
# Dữ liệu tra theo TCVN1651-1:2008 và TCVN1651-2:2008
betong_Rb = {
    "B15": 8.5, "B20": 11.5, "B25": 14.5, "B30": 17.0, "B35": 19.5,
    "B40": 22.0, "B45": 25.0, "B50": 27.5, "B55": 30.0, "B60": 33.0,
    "B70": 37.0, "B80": 41.0, "B90": 44.0, "B100": 47.5
}

thep_Rs = {
    "CB240-T": (210, 210), "CB300-T": (260, 260), "CB300-V": (260, 260),
    "CB400-V": (350, 365), "CB500-V": (435, 435)
}

thep_tron = {
    6: 28.3, 8: 50.3, 10: 78.5, 12: 113.1, 14: 153.9, 16: 201.1,
    18: 254.5, 20: 314.2, 22: 380.1, 25: 490.9, 28: 615.8,
    32: 804.2, 36: 1017.9, 40: 1256.6
}

bang_xi_alpha_R = {
    "CB240-T": {"<=B60": {"xiR": 0.615, "alphaR": 0.426}, "B70": {"xiR": 0.531, "alphaR": 0.390},
                "B80": {"xiR": 0.523, "alphaR": 0.386}, "B90": {"xiR": 0.517, "alphaR": 0.383},
                "B100": {"xiR": 0.509, "alphaR": 0.380}},
    "CB300-T": {"<=B60": {"xiR": 0.583, "alphaR": 0.413}, "B70": {"xiR": 0.502, "alphaR": 0.376},
                "B80": {"xiR": 0.494, "alphaR": 0.372}, "B90": {"xiR": 0.487, "alphaR": 0.368},
                "B100": {"xiR": 0.478, "alphaR": 0.364}},
    "CB300-V": {"<=B60": {"xiR": 0.583, "alphaR": 0.413}, "B70": {"xiR": 0.502, "alphaR": 0.376},
                "B80": {"xiR": 0.494, "alphaR": 0.372}, "B90": {"xiR": 0.487, "alphaR": 0.368},
                "B100": {"xiR": 0.478, "alphaR": 0.364}},
    "CB400-V": {"<=B60": {"xiR": 0.533, "alphaR": 0.391}, "B70": {"xiR": 0.457, "alphaR": 0.353},
                "B80": {"xiR": 0.448, "alphaR": 0.348}, "B90": {"xiR": 0.440, "alphaR": 0.343},
                "B100": {"xiR": 0.431, "alphaR": 0.338}},
    "CB500-V": {"<=B60": {"xiR": 0.481, "alphaR": 0.365}, "B70": {"xiR": 0.411, "alphaR": 0.326},
                "B80": {"xiR": 0.401, "alphaR": 0.320}, "B90": {"xiR": 0.390, "alphaR": 0.316},
                "B100": {"xiR": 0.380, "alphaR": 0.309}}
}


def tra_xi_alpha_R(loai_thep, cap_betong):
    if loai_thep not in bang_xi_alpha_R:
        raise ValueError("Loại thép không hợp lệ.")
    if cap_betong in bang_xi_alpha_R[loai_thep]:
        return bang_xi_alpha_R[loai_thep][cap_betong]
    else:
        return bang_xi_alpha_R[loai_thep]["<=B60"]


def tinh_As(M, b, h, Rb, Rs, xi, alphaR):
    a = 50
    h0 = h - a
    alphaM = M * 1000 / (Rb * 10 ** -3 * b * h0 ** 2)

    if alphaM > alphaR:
        print(f"Bài toán cốt kép vì alphaM = {round(alphaM, 3)} > alphaR = {alphaR}")
        return None
    else:
        xi = 1 - math.sqrt(1 - 2 * alphaM)
    As = xi * Rb * b * h0 / Rs
    return round(As, 1)


def chon_3_phuong_an_thep(As_req):
    ket_qua = []

    for d, As1 in thep_tron.items():
        for n in range(1, 6):
            tong_As = round(n * As1, 1)
            if tong_As >= As_req:
                ket_qua.append((n, d, tong_As, abs(tong_As - As_req)))
                break

    ket_qua = sorted(ket_qua, key=lambda x: x[3])

    return ket_qua[:3]


print("==> Nhập các thông số thiết kế:")
M = float(input("Mômen uốn M (kNm): "))
b = float(input("Chiều rộng dầm b (mm): "))
h = float(input("Chiều cao dầm h (mm): "))

cap_betong = input("Cấp độ bền bê tông (vd B20): ").upper().strip()
loai_thep = input("Loại thép (vd CB300-V): ").upper().strip()

if cap_betong not in betong_Rb:
    raise ValueError("Cấp độ bền bê tông không hợp lệ.")
if loai_thep not in thep_Rs:
    raise ValueError("Loại thép không hợp lệ.")

Rb = betong_Rb[cap_betong]
Rs, Rsc = thep_Rs[loai_thep]
xi_alpha = tra_xi_alpha_R(loai_thep, cap_betong)
xiR = xi_alpha["xiR"]
alphaR = xi_alpha["alphaR"]

As = tinh_As(M, b, h, Rb, Rs, xiR, alphaR)

if As:
    print(f"\nRb = {Rb} MPa | Rs = {Rs} MPa | Rsc = {Rsc} MPa")
    print(f"Diện tích cốt thép yêu cầu As = {As} mm²")

    phuong_an = chon_3_phuong_an_thep(As)
    print(f"\nGợi ý 3 phương án chọn thép:")
    for i, (sl, phi, tong_As, _) in enumerate(phuong_an, 1):
        print(f"{i}. {sl}Φ{phi} → {tong_As} mm²")
else:
    print("Bài toán cốt kép.")loading BTCT.py…]()







