# ĐỀ BÀI
Giới hạn thời gian: `1s`

Giới hạn bộ nhớ: `256 MB`

Cho 1 số nguyên 32-bit không âm, ban đầu tất cả các bit đều bằng 0. Ta có 3 loại truy vấn:

- `1 k`: Gán bit thứ $k$ = 1.
- `2 k`: Gán bit thứ $k$ = 0.
- `3 k`: Đảo bit thứ $k$ (nếu bit thứ $k$ = 1 --> gán = 0 và ngược lại).

In ra số nguyên sau mỗi truy vấn.

# INPUT
- Dòng đầu tiên chứa 1 số nguyên $q$ ($1 \le q \le 10^5$).
- $q$ dòng tiếp theo, mỗi dòng chứa 2 số nguyên $c, k$ ($1 \le c \le 3$, $0 \le k \le 31$).

# OUTPUT
In ra số nguyên sau mỗi truy vấn.

# EXAMPLE
INPUT:
```text
5
1 0
1 1
2 0
3 1
3 3
```

OUTPUT:
```text
1
3
2
0
8
```

# KIẾN THỨC MỚI
Các thao tác cơ bản trên bit thứ k (**LƯU Ý**: Nên dùng `1U` để tránh tràn bit dấu):
```
- Gán bit = 1: `x |= (1U << k)`
- Gán bit = 0: `x &= ~(1U << k)`
- Đảo bit: `x ^= (1U << k)`
```

# LỜI GIẢI
Sử dụng các phép toán ở phần trên để xử lý từng loại truy vấn.

**Độ phức tạp:** $O(1)$ mỗi truy vấn.

**LƯU Ý**: Vì số nguyên của ta **KHÔNG ÂM**, ta nên để kiểu dữ liệu `unsigned int` và dùng `1U` khi thao tác bit shift.

<details>
<summary><b>Code mẫu (C++)</b></summary>

```cpp
void solve(){
    int q; cin >> q;
    unsigned int x = 0;
    for(int t = 0; t < q; t++){
        int c, k; cin >> c >> k;
        if(c == 1) x |= (1U << k);
        else if(c == 2) x &= ~(1U << k);
        else x ^= (1U << k);
        cout << x << '\n';
    }
}
```
</details>


