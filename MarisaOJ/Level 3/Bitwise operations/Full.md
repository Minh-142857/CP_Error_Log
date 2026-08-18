# ĐỀ BÀI
Giới hạn thời gian: `1000 ms`

Giới hạn bộ nhớ: `256 MB`

Cho $n$ xâu (chỉ chứa chữ cái tiếng Anh thường), đếm số cách chọn một tập con trong $n$ xâu đó sao cho mỗi chữ cái xuất hiện ít nhất 1 lần.

2 cách được xem là khác nhau nếu có ít nhất 1 xâu thuộc cách này mà không thuộc cách kia.

# INPUT
* Dòng đầu tiên chứa số nguyên $n$ $(1 \le n \le 25)$.
* $n$ dòng tiếp theo, mỗi dòng chứa 1 xâu $S$ ($1 \le |S| \le 50$, với $|S|$ là độ dài xâu $S$).

# OUTPUT
In ra một số nguyên duy nhất là số cách thỏa mãn điều kiện đề bài.

# EXAMPLE
INPUT:
```text
8
the
quick
brown
fox
jumps
over
lazy
dog
```

OUTPUT:
```text
1
```

# KIẾN THỨC MỚI
**State Compression (Nén trạng thái):** Thay vì dùng mảng `bool` hoặc mảng đếm tần suất để quản lý sự xuất hiện của các phần tử nhỏ, ta có thể dùng một số nguyên 32-bit. Mỗi bit của số nguyên đại diện cho trạng thái Có/Không của một phần tử.

# GỢI Ý
<details>
<summary><b>Gợi ý 1</b></summary>

- Giới hạn của $n$ đang báo hiệu cho ta dùng thuật toán gì?

- Không gian ta cần kiểm tra là gì, có số lượng bao nhiêu?  

- Từ câu hỏi trên, kỹ thuật **Nén trạng thái** ở phần trên có thể áp dụng trong bài toán này không?

</details>

<details>
<summary><b>Gợi ý 2</b></summary>

- Làm sao để ta "mã hóa" 1 xâu thành các chỉ số, để có thể "nén" xâu thành 1 số nguyên? (Bảng chữ cái sẽ giúp ích.)

- Sau khi đã "mã hóa", làm sao để cập nhật trạng thái hiện tại? (Ta đã làm ở các bài trước.)

</details>

<details>
<summary><b>Gợi ý 3</b></summary>

Khi cả 26 chữ cái đều xuất hiện, giá trị của nó sau khi "mã hóa" là gì?

</details>

# LỜI GIẢI
<details>
<summary><b>Lời giải</b></summary>

Vì $n \le 25$, ta hoàn toàn có thể dùng **Backtracking** để sinh toàn bộ khả năng. Tuy nhiên, nếu ta "ngây thơ" sử dụng mảng đếm (hoặc mảng `bool`) để cập nhật mỗi lần, code chắc chắn sẽ bị đẩy độ phức tạp lên $O(2^n * |S|)$, làm code bị `TLE`.

<details>
<summary><b>Code ngây thơ (C++)</b></summary>

```cpp
int n;
vector<string> s;
int ans = 0;
 
void backtrack(int x, vector<int> &cnt){
    if(x == n){
        bool check = true;
        for(int i = 0; i < 26; i++) if(cnt[i] == 0){
            check = false;
            break;
        }
        if(check) ans++;
        return;
    }
    backtrack(x + 1, cnt);
    for(char c : s[x]) cnt[c - 'a']++;
    backtrack(x + 1, cnt);
    for(char c : s[x]) cnt[c - 'a']--;
}
 
void solve(){
    cin >> n;
    s.resize(n);
    for(int i = 0; i < n; i++) cin >> s[i];
    vector<int> a(26, 0);
    backtrack(0, a);
    cout << ans;
}
```

</details>

Ta nhận thấy không gian kiểm tra của ta **chỉ có 26 chữ cái tiếng Anh**. Do đó, ta có thể sử dụng kỹ thuật **Nén trạng thái** để tối ưu.

Với mỗi xâu, ta sẽ "mã hóa" từng ký tự theo thứ tự của nó trên bảng chữ cái (tính từ 0), và mỗi con số sẽ quyết định bit nào sẽ được bật.

VD: Xâu "the" có thể được mã hóa thành 19 7 4, có nghĩa là bit 19, bit 7 và bit 4 sẽ được bật.

Sau khi "mã hóa" các xâu thành số nguyên, mỗi lượt cập nhật trạng thái chỉ mất $O(1)$ với công thức `s = s | a[x]`.

Công việc còn lại là Backtracking trên các số nguyên. Nếu cả 26 chữ cái đều đã xuất hiện (đồng nghĩa với tất cả các bit từ 0 đến 25 đều được bật), tăng `ans` thêm 1.

**Độ phức tạp:** $O(2^n)$, hoàn toàn có thể chạy trong `1s`.

</details>

<details>
<summary><b>Code mẫu (C++)</b></summary>

```cpp
int n;
vector<int> a;
int ans = 0, target = (1 << 26) - 1;

void backtrack(int x, int s){
    if(x == n){
        if(s == target) ans++;
        return;
    }
    backtrack(x + 1, s | a[x]);
    backtrack(x + 1, s);
}

void solve(){
    cin >> n;
    a.resize(n);
    for(int i = 0; i < n; i++){
        string s; cin >> s;
        for(char c : s) a[i] |= (1 << (c - 'a'));
    }
    backtrack(0, 0);
    cout << ans;
}
```

</details>

# BÀI HỌC RÚT RA
* **Giới hạn $N \le 25$:** Đây là một giới hạn rất nhạy cảm. $2^{25}$ đủ nhỏ để duyệt Backtracking, nhưng BẮT BUỘC mỗi bước chuyển trạng thái (State Transition) phải là $O(1)$. 
* **Sức mạnh của phần cứng:** Việc dùng phép `|` (Bitwise OR) thay cho vòng lặp duyệt mảng là chìa khóa sống còn giúp giải phóng sức mạnh của ALU (Bộ số học và logic) trong CPU, giảm Memory Overhead đáng kể.