---
title: "CF 1108C - Vòng Hoa Đẹp"
description: "Chúng ta được cung cấp một dòng đèn, mỗi chiếc được sơn một trong ba màu: R, G hoặc B. Chúng ta được phép sơn lại bất kỳ chiếc đèn nào thành bất kỳ màu nào khác."
date: "2026-06-12T05:21:04+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1108
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 535 (Div. 3)"
rating: 1300
weight: 1108
solve_time_s: 424
verified: false
draft: false
---

[CF 1108C - Vòng hoa đẹp](https://codeforces.com/problemset/problem/1108/C) 

**Đánh giá:** 1300 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 7 phút 4 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dòng đèn, mỗi chiếc được sơn một trong ba màu: R, G hoặc B. Chúng ta được phép sơn lại bất kỳ chiếc đèn nào thành bất kỳ màu nào khác. Mục đích là sửa đổi trình tự sao cho nó trở nên “định kỳ theo nghĩa hình học cụ thể”: bất kỳ hai đèn nào có cùng màu cuối cùng phải đặt ở các vị trí có chỉ số khác nhau bội số của 3. 

Tình trạng này có một hậu quả cấu trúc rất cứng nhắc. Nếu chúng ta xem xét các vị trí modulo 3, chẳng hạn như các chỉ số 0, 1, 2 lặp lại, thì tất cả các vị trí có cùng màu phải thuộc chính xác một trong các lớp dư lượng này. Nói cách khác, mỗi lớp dư mod 3 phải được gán chính xác một màu và phép gán đó phải nhất quán trên toàn bộ chuỗi. 

Vì vậy, nhiệm vụ trở thành việc chọn ánh xạ từ ba lớp dư lượng {0,1,2} đến ba màu {R,G,B}, đảm bảo mỗi màu được sử dụng chính xác một lần và sau đó sơn lại từng vị trí để khớp với màu được gán cho lớp dư lượng của nó. Chúng tôi muốn giảm thiểu số lượng vị trí khác với chuỗi gốc. 

Kích thước đầu vào có thể lên tới 200.000. Điều đó ngay lập tức loại trừ bất cứ điều gì bậc hai hoặc bậc ba về số lượng vị trí. Bất kỳ giải pháp nào thử tất cả các kết hợp sơn lại cục bộ cho mỗi vị trí một cách độc lập mà không có cấu trúc sẽ quá chậm hoặc không chính xác, bởi vì ràng buộc kết hợp các vị trí trên toàn cầu thông qua các lớp modulo. 

Một trường hợp thất bại tinh tế đối với lối suy nghĩ ngây thơ là thử các cách sửa chữa cục bộ tham lam như “sơn từng vị trí để phù hợp với màu sắc đa số giữa các hàng xóm” hoặc các phương pháp phỏng đoán tương tự. Ví dụ, với đầu vào`RGBRGB`, một cách tiếp cận tham lam có thể sớm thay đổi vị trí một cách không chính xác và phá vỡ tính nhất quán toàn cầu, ngay cả khi giải pháp tối ưu đã có hiệu lực. 

Một trường hợp tinh tế khác là giả sử một mẫu cố định như “RGBRGB…” luôn hoạt động tốt nhất. Vì`BRB`, một mẫu cố định như vậy có thể yêu cầu nhiều thay đổi hơn so với một hoán vị màu khác được gán cho phần dư. 

Khó khăn cốt lõi là vấn đề không nằm ở vị trí riêng lẻ mà là ở việc chọn hoán vị tốt nhất của phép gán 3 màu trên 3 lớp dư lượng. 

## Phương pháp tiếp cận 

Một cái nhìn mạnh mẽ làm cho cấu trúc trở nên rõ ràng. Vì mỗi vòng hoa cuối cùng hợp lệ được xác định bằng cách gán một hoán vị của {R, G, B} cho các lớp dư 0, 1 và 2, nên chúng ta có thể thử cả 3! = 6 bài tập. Đối với mỗi nhiệm vụ, chúng tôi tính toán có bao nhiêu vị trí đã phù hợp và chọn vị trí tốt nhất. 

Đối với một phép gán cố định, chúng tôi quét chuỗi một lần và đếm những phần không khớp. Chi phí này là O (n). Với 6 hoán vị, tổng số là O(6n), là tuyến tính và rất nhỏ trong thực tế. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng điều kiện “khoảng cách chia hết cho 3” hoàn toàn tương đương với việc nhóm các chỉ số theo modulo 3. Khi đã thấy điều đó, vấn đề không còn là việc đổi màu cấu trúc tùy ý nữa mà trở thành vấn đề gán kích thước không đổi trên các lớp dư lượng. 

Không cần lập trình động hoặc mô phỏng tham lam trên chuỗi; toàn bộ không gian tối ưu hóa được chia thành sáu khả năng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (6 hoán vị) | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu (cùng ý tưởng, có cấu trúc) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia các chỉ số của chuỗi thành ba nhóm dựa trên i mod 3. 

Đây là ràng buộc cấu trúc duy nhất trong bài toán, vì vậy chúng tôi cô lập nó ngay lập tức. 
2. Tính toán trước số lượng R, G và B xuất hiện trong mỗi loại dư lượng. 

Điều này tránh việc tính toán lại các so sánh ký tự nhiều lần cho mỗi hoán vị. 
3. Liệt kê tất cả các hoán vị của ba màu. 

Mỗi hoán vị thể hiện sự gán màu hợp lệ đầy đủ cho các lớp dư lượng. 
4. Đối với một hoán vị nhất định, hãy tính xem có bao nhiêu ký tự đã khớp: 

đối với mỗi loại dư lượng r, hãy cộng số lượng màu mục tiêu được gán cho r. 
5. Theo dõi hoán vị với số lượng khớp tối đa. 

Tối đa hóa các kết quả phù hợp tương đương với việc giảm thiểu việc đổi màu. 
6. Xây dựng lại chuỗi câu trả lời bằng cách sử dụng phép gán tốt nhất bằng cách viết màu đã chọn cho mỗi vị trí i dựa trên i mod 3. 

### Tại sao nó hoạt động 

Mỗi chuỗi “đẹp” hợp lệ phải gán chính xác một màu cho mỗi lớp dư lượng modulo 3, bởi vì nếu hai vị trí có chung một màu, thì chỉ số của chúng phải khác nhau bội số của 3, điều này buộc chúng vào cùng một lớp dư lượng. Vì có chính xác ba màu và ba lớp cặn nên phép gán này là một phép gán. Thuật toán liệt kê tất cả các phép đối chiếu và chọn một phép đối sánh phù hợp nhất với chuỗi gốc, đảm bảo tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    s = input().strip()

    colors = ['R', 'G', 'B']

    # cnt[pos_mod][color_index]
    cnt = [[0] * 3 for _ in range(3)]

    idx = {'R': 0, 'G': 1, 'B': 2}

    for i, ch in enumerate(s):
        cnt[i % 3][idx[ch]] += 1

    best_perm = None
    best_keep = -1

    # try all permutations
    from itertools import permutations

    for perm in permutations(range(3)):
        keep = 0
        for r in range(3):
            keep += cnt[r][perm[r]]
        if keep > best_keep:
            best_keep = keep
            best_perm = perm

    res = list(s)
    for i in range(n):
        res[i] = colors[best_perm[i % 3]]

    print(n - best_keep)
    print("".join(res))

if __name__ == "__main__":
    solve()
```Việc triển khai nén vấn đề thành việc đếm các kết quả phù hợp trên mỗi lớp dư lượng. các`cnt`bảng lưu trữ tần suất mỗi màu xuất hiện trong mỗi nhóm modulo. Mỗi hoán vị biểu thị một ánh xạ từ lớp dư lượng đến chỉ số màu. Điểm của một hoán vị chỉ đơn giản là có bao nhiêu vị trí đã khớp với ánh xạ đó. 

Một cạm bẫy phổ biến là việc xây dựng lại chuỗi ứng cử viên trong quá trình đánh giá từng hoán vị. Điều đó là không cần thiết; chúng tôi chỉ tính điểm. Việc xây dựng lại được thực hiện một lần bằng cách sử dụng hoán vị tốt nhất. 

Một điểm tinh tế khác là đảm bảo rằng ánh xạ được áp dụng theo chỉ số modulo 3, không phải theo vị trí trong vòng lặp hoán vị. Việc trộn lẫn hai cách giải thích đó thường dẫn đến những câu trả lời sai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
BRB
```Chúng tôi tính toán các lớp dư lượng: 

| tôi mod 3 | nhân vật | 
| --- | --- | 
| 0 | B | 
| 1 | R | 
| 2 | B | 

Đếm: 

| mod | R | G | B | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 1 | 
| 1 | 1 | 0 | 0 | 
| 2 | 0 | 0 | 1 | 

Bây giờ hãy thử hoán vị, ví dụ (G, R, B): 

| mod | được giao | trận đấu | 
| --- | --- | --- | 
| 0 | G | 0 | 
| 1 | R | 1 | 
| 2 | B | 1 | 

Tổng số trận đấu = 2, vì vậy chỉ cần 1 thay đổi. 

Kết quả trở thành:```
GRB
```Điều này khớp với mẫu và xác nhận rằng sự phân bổ tối ưu phụ thuộc vào sự phân bổ giữa các lớp dư lượng. 

### Ví dụ 2 

đầu vào:```
6
RRGGBB
```Các lớp dư lượng: 

| mod | ký tự | 
| --- | --- | 
| 0 | R, G | 
| 1 | R, B | 
| 2 | G, B | 

Một hoán vị tốt là (R, G, B): 

| mod | được giao | trận đấu | 
| --- | --- | --- | 
| 0 | R | 1 | 
| 1 | G | 0 | 
| 2 | B | 1 | 

Tổng số trận đấu = 2, vì vậy cần 4 thay đổi. 

Ví dụ này cho thấy rằng ngay cả các chuỗi trông đối xứng cũng có thể yêu cầu nhiều lần đổi màu vì sự phân bổ trên các lớp dư lượng không được căn chỉnh theo một mẫu tuần hoàn rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét chuỗi một lần để xây dựng số lượng, sau đó kiểm tra 6 hoán vị, mỗi hoán vị O(1) hoạt động trên 3 lớp cố định | 
| Không gian | O(1) | Chỉ sử dụng bảng tần số 3x3 và các mảng không đổi | 

Quét tuyến tính dễ dàng phù hợp với các ràng buộc cho n lên tới 200.000. Việc sử dụng bộ nhớ là không đổi và không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io
from itertools import permutations

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from subprocess import Popen, PIPE

    # simple inline execution via import trick
    # assume solution is in solve()
    # we redefine here for testing convenience

    def solve():
        n = int(input().strip())
        s = input().strip()
        colors = ['R', 'G', 'B']
        idx = {'R':0,'G':1,'B':2}
        cnt = [[0]*3 for _ in range(3)]
        for i,ch in enumerate(s):
            cnt[i%3][idx[ch]] += 1

        best = -1
        bestp = None
        for p in permutations(range(3)):
            keep = sum(cnt[r][p[r]] for r in range(3))
            if keep > best:
                best = keep
                bestp = p

        res = []
        for i in range(n):
            res.append(colors[bestp[i%3]])
        print(n-best)
        print("".join(res))

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# provided sample
assert run("3\nBRB\n") == "1\nGRB"

# all same
assert run("4\nRRRR\n") in ["2\nRGRG", "2\nGRGR"]

# already optimal
assert run("3\nRGB\n") == "0\nRGB"

# alternating
assert run("6\nRGRGRG\n") in ["0\nRGRGRG"]

# mixed
assert run("5\nBRRGB\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 BRB | 1 GR | tính đúng đắn cơ bản | 
| 4 RRR | 2 RGRG hoặc GRGR | đối xứng và nhiều câu trả lời tối ưu | 
| 3 RGB | 0 RGB | cấu hình đã hợp lệ | 
| 6 RRGRG | 0 RGRGR | trường hợp căn chỉnh định kỳ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi chuỗi đã hoàn toàn tuần hoàn. Đối với đầu vào`RGBRGB`, mỗi lớp dư lượng đã khớp với một màu riêng biệt. Thuật toán đếm các kết quả khớp đầy đủ cho một hoán vị, dẫn đến không có thay đổi nào. Vì chúng ta chỉ so sánh các hoán vị nên chúng ta bảo toàn cấu trúc ban đầu một cách tự nhiên. 

Một trường hợp cạnh khác là đầu vào thống nhất như`RRRRRR`. Ở đây mỗi lớp dư lượng có sự phân bố giống hệt nhau, do đó nhiều hoán vị mang lại cùng một số điểm. Thuật toán chọn bất kỳ hoán vị nào một cách nhất quán, tạo ra một mẫu xen kẽ hợp lệ, chẳng hạn như`RGRGBB`hoặc một phương án tối ưu tương đương khác tùy theo trình tự thực hiện. Tính đúng đắn không phụ thuộc vào việc chọn cà vạt nào. 

Trường hợp thứ ba là khi một lớp cặn chiếm ưu thế hơn một màu cụ thể nhưng không độc quyền. Thuật toán vẫn gán chính xác màu đó cho lớp modulo tương ứng vì tính điểm hoán vị nắm bắt mức tối đa toàn cầu trên tất cả các phép gán thay vì đưa ra quyết định cục bộ.
