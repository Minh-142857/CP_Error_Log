# ĐỀ BÀI
Giới hạn thời gian: `1000 ms`

Giới hạn bộ nhớ: `256 MB`

Cho một mảng $A$ gồm $n$ số nguyên không âm. Đếm số lượng cặp $i < j$ sao cho $A_i \oplus j = A_j \oplus i$, trong đó $\oplus$ là phép toán bitwise XOR.

# INPUT
* Dòng đầu tiên chứa số nguyên $n$ $(1 \le n \le 10^5)$.
* Dòng thứ hai chứa $n$ số nguyên không âm $A_i$ $(1 \le A_i \le 10^9)$.

# OUTPUT
In ra một số nguyên duy nhất là số lượng cặp thỏa mãn điều kiện đề bài.

# EXAMPLE
INPUT:
```text
5
7 8 9 14 3
```

OUTPUT:
```text
4
```

# KIẾN THỨC MỚI
**Tính chất của phép XOR ($\oplus$):**
- Giao hoán: $A \oplus B = B \oplus A$
- Kết hợp: $(A \oplus B) \oplus C = A \oplus (B \oplus C)$
- Tự nghịch đảo: $A \oplus A = 0$
- Phép "Chuyển vế": $X \oplus Y = Z \Leftrightarrow X \oplus Z = Y$

# GỢI Ý
<details>
<summary><b>Gợi ý 1</b></summary>

Sử dụng các phép toán ở phần trên, hãy tìm cách cô lập $i, j$ về 2 phía.

</details>

# LỜI GIẢI
<details>
<summary><b>Lời giải</b></summary>

Ta biến đổi biểu thức ban đầu như sau:

$$
\begin{aligned}
a[i] \oplus j = a[j] \oplus i &\Leftrightarrow a[i] \oplus (a[j] \oplus i) = j \\\\
&\Leftrightarrow a[i] \oplus (i \oplus a[j]) = j \\\\
&\Leftrightarrow (a[i] \oplus i) \oplus a[j] = j \\\\
&\Leftrightarrow a[j] \oplus (a[i] \oplus i) = j \\\\
&\Leftrightarrow a[j] \oplus j = a[i] \oplus i
\end{aligned}
$$

$\Rightarrow$ Bài toán trở thành tìm số cặp $(i, j)$ sao cho $C[i] = C[j]$, với $C[x] = a[x] \oplus x$.

Ta có thể sử dụng `std::map` hoặc `std::unordered_map` để đếm tần suất trong $O(\log N)$.

**Độ phức tạp:** $O(N \log N)$.

</details>

<details>
<summary><b>Code mẫu (C++)</b></summary>

```cpp
void solve(){
    lint n; cin >> n;
    map<lint, lint> cnt;
    for(int i = 1; i <= n; i++){
        lint x; cin >> x;
        cnt[x ^ i]++;
    }
    lint ans = 0;
    for(auto it : cnt){
        lint f = it.second;
        ans += f * (f - 1) / 2;
    }
    cout << ans;
}
```

</details>

# BÀI HỌC RÚT RA
* **Cô lập biến (Variable Isolation):** Khi bài toán yêu cầu tìm cặp $(i, j)$ thỏa mãn một phương trình, nếu có thể dùng đại số dồn toàn bộ $i$ sang một vế, $j$ sang một vế $\rightarrow$ Bài toán $O(N^2)$ chắc chắn có thể giải trong $O(N \log N)$ bằng cách đếm tần suất.
* **Tràn số Tổ hợp:** Nếu mọi phần tử trong mảng đều sinh ra cùng một giá trị $X$, số cặp tối đa là $\frac{10^5 \times (10^5 - 1)}{2} \approx 5 \times 10^9$. Bắt buộc phải dùng `long long` cho đáp án `ans` và biến tần suất `f` để tránh Integer Overflow.