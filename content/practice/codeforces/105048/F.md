---
title: "CF 105048F - Phát minh từ ngữ"
description: "Chúng ta được cho một chuỗi có độ dài $n$. Chúng ta được phép áp dụng nhiều lần thao tác hoán đổi để trao đổi các ký tự ở vị trí $i$ và $i+k$, miễn là cả hai chỉ số đều hợp lệ. Những giao dịch hoán đổi này có thể được thực hiện nhiều lần theo bất kỳ thứ tự nào."
date: "2026-06-28T05:43:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105048
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 03-22-24 Div. 2 (Beginner)"
rating: 0
weight: 105048
solve_time_s: 91
verified: false
draft: false
---

[CF 105048F - Phát minh từ ngữ](https://codeforces.com/problemset/problem/105048/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi có độ dài$n$. Chúng ta được phép áp dụng nhiều lần thao tác hoán đổi hoán đổi ký tự tại các vị trí$i$Và$i+k$, miễn là cả hai chỉ số đều hợp lệ. Những giao dịch hoán đổi này có thể được thực hiện nhiều lần theo bất kỳ thứ tự nào. 

Nhiệm vụ là xác định có thể tạo ra bao nhiêu chuỗi riêng biệt từ chuỗi gốc bằng cách áp dụng bất kỳ chuỗi hoán đổi nào. Hai chuỗi được coi là khác nhau nếu có ít nhất một vị trí khác nhau. 

Cấu trúc chính là các giao dịch hoán đổi không hoạt động trên toàn cầu. Mỗi lần hoán đổi chỉ kết nối các vị thế có chỉ số khác nhau một cách chính xác.$k$. Điều này ngay lập tức gợi ý rằng các chỉ số được phân chia thành các nhóm độc lập dựa trên modulo còn lại của chúng.$k$. 

Ràng buộc$n \le 10^6$ngụ ý bất kỳ giải pháp nào về cơ bản phải là tuyến tính hoặc$O(n \log n)$. Bất cứ điều gì liên quan đến việc liệt kê các hoán vị hoặc BFS trên các trạng thái đều không thể thực hiện được vì không gian trạng thái là giai thừa trong$n$. Chúng tôi buộc phải nén tác động của các hoán đổi thành một bài toán đếm tổ hợp thay vì mô phỏng các phép biến đổi. 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ là giả định rằng bất kỳ hoán vị nào của chuỗi đều có thể xảy ra nếu tồn tại đủ số lần hoán đổi. Ví dụ, với$n=4, k=2$, chuỗi “aabb” không thể tạo ra cả 4! hoán vị. Chỉ các vị trí (1,3) và (2,4) được kết nối nên các ký tự chỉ có thể được sắp xếp lại trong các cặp đó. Một mô hình hoán vị toàn cục ngây thơ được đếm quá mức một cách ồ ạt. 

Một trường hợp cạnh khác là khi$k=1$. Khi đó, mọi hoán đổi liền kề đều được phép, có nghĩa là toàn bộ chuỗi có thể được hoán vị tùy ý, vì vậy câu trả lời là$n!$chia cho nhiều ký tự. 

Ở một thái cực khác, khi$k=n$, không thể hoán đổi được nên chỉ tồn tại một cấu hình. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ mô hình hóa từng chỉ mục dưới dạng một nút và thêm các cạnh vào giữa$i$Và$i+k$. Sau đó, chúng tôi sẽ xây dựng các thành phần được kết nối và mô phỏng tất cả các hoán vị có thể truy cập được trong mỗi thành phần. Mỗi thành phần có kích thước$m$sẽ đóng góp$m!$sắp xếp lại, nhưng các bản sao phải được loại bỏ do các ký tự lặp lại. Điều này dẫn đến các hệ số đa thức cho mỗi thành phần. 

Tuy nhiên, brute-force không thành công vì việc tạo ra tất cả các hoán vị bên trong các thành phần một cách rõ ràng là theo cấp số nhân. Ngay cả việc xây dựng một biểu đồ trạng thái cũng không khả thi vì mỗi thành phần riêng lẻ có thể có kích thước$O(n)$, tạo ra sự tăng trưởng giai thừa. 

Quan sát quan trọng là các giao dịch hoán đổi chỉ cho phép sắp xếp lại tùy ý trong mỗi thành phần được kết nối được hình thành bằng cách tăng từng bước của$k$. Các thành phần này chính xác là các lớp dư lượng modulo$\gcd(n,k)$cấu trúc gây ra bởi đồ thị$i \leftrightarrow i+k$. Trên thực tế, các chỉ số được chia thành$k$chuỗi độc lập:$$i, i+k, i+2k, \dots$$trong giới hạn. Trong mỗi chuỗi, bất kỳ hoán vị nào cũng có thể đạt được vì các hoán đổi liền kề dọc theo chuỗi sẽ tạo ra nhóm đối xứng đầy đủ trên thành phần đó. 

Do đó, vấn đề rút gọn thành: đối với mỗi chuỗi, hãy đếm xem có bao nhiêu hoán vị riêng biệt của nhiều tập ký tự của nó tồn tại. Đây là số lượng đa thức và vì các chuỗi là độc lập nên chúng tôi nhân kết quả trên tất cả các chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | hàm mũ | hàm mũ | Quá chậm | 
| Đếm Thành Phần + Đa ​​Thức |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Phân chia chỉ số thành chuỗi 

Chúng tôi nhóm các vị trí theo modulo dư lượng của chúng$k$. Đối với mỗi phần còn lại$r$, xét chỉ số$r, r+k, r+2k, \dots$. Mỗi nhóm như vậy là độc lập vì việc hoán đổi không bao giờ di chuyển các ký tự giữa các nhóm khác nhau. Đây là sự tách rời cơ bản của vấn đề. 

### Bước 2: Đếm số ký tự trong mỗi chuỗi 

Đối với mỗi chuỗi, chúng tôi thu thập nhiều tập hợp ký tự xuất hiện ở các vị trí đó. Vì bất kỳ hoán vị nào bên trong chuỗi đều có thể truy cập được nên số cách sắp xếp riêng biệt chỉ phụ thuộc vào bội số. 

### Bước 3: Tính toán đóng góp đa thức cho mỗi chuỗi 

Nếu một chuỗi có chiều dài$m$và số lượng ký tự$c_1, c_2, \dots$thì số hoán vị phân biệt là:$$\frac{m!}{\prod c_i!}$$Chúng tôi tính toán giai thừa và nghịch đảo mô-đun để đánh giá điều này một cách hiệu quả. 

Lý do điều này hợp lệ là vì các giao dịch hoán đổi tạo ra tất cả các chuyển vị liền kề dọc theo chuỗi, tạo ra nhóm đối xứng đầy đủ trên chuỗi đó. 

### Bước 4: Nhân các khoản đóng góp 

Vì các chuỗi là độc lập nên tổng số chuỗi có thể truy cập được là tích của tất cả các chuỗi. 

### Tại sao nó hoạt động 

Mỗi lần hoán đổi bảo toàn phân vùng các chỉ số theo modulo dư$k$, vì vậy không có nhân vật nào rời khỏi chuỗi của nó. Bên trong một chuỗi, các giao dịch hoán đổi tạo thành một biểu đồ đường dẫn được kết nối và các giao dịch hoán đổi liền kề tạo ra tất cả các hoán vị của đường dẫn đó. Do đó, mỗi chuỗi đóng góp chính xác số lượng hoán vị của nhiều tập hợp của nó, độc lập với các chuỗi khác. Độc lập hàm ý phép nhân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n, k = map(int, input().split())
    s = input().strip()

    fact = [1] * (n + 1)
    invfact = [1] * (n + 1)

    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact[n] = modinv(fact[n])
    for i in range(n, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    used = [False] * n
    ans = 1

    for r in range(k):
        freq = [0] * 26
        length = 0
        i = r
        while i < n:
            freq[ord(s[i]) - 97] += 1
            length += 1
            i += k

        ways = fact[length]
        for c in freq:
            ways = ways * invfact[c] % MOD

        ans = ans * ways % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách tính toán trước các giai thừa và các giai thừa nghịch đảo lên đến$n$, vì mọi chuỗi đều yêu cầu hệ số đa thức. Nghịch đảo mô đun được tính toán bằng định lý Fermat, định lý này hợp lệ vì mô đun là số nguyên tố. 

Vòng lặp chính lặp qua$k$các lớp dư lượng. Đối với mỗi lớp, chúng tôi duyệt qua các chỉ số cách nhau bởi$k$, tích lũy tần số ký tự. Công thức đa thức được áp dụng trực tiếp bằng bảng giai thừa. 

Một cạm bẫy phổ biến là quên rằng mỗi chuỗi phải được xử lý độc lập; tôi
