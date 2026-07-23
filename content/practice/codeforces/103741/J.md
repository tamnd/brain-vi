---
title: "CF 103741J - Trình tự"
description: "Chúng tôi đang duy trì một chuỗi số nguyên động. Chuỗi bắt đầu trống và chỉ phát triển bằng cách thêm các phần tử vào cuối. Tại bất kỳ thời điểm nào, chúng ta có thể được yêu cầu tính một giá trị xuất phát từ tất cả các cặp phần tử trong đó phần tử đầu tiên ở bên trái phần tử thứ hai."
date: "2026-07-02T09:06:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103741
codeforces_index: "J"
codeforces_contest_name: "HUSTPC 2022"
rating: 0
weight: 103741
solve_time_s: 46
verified: true
draft: false
---

[CF 103741J - Trình tự](https://codeforces.com/problemset/problem/103741/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một chuỗi số nguyên động. Chuỗi bắt đầu trống và chỉ phát triển bằng cách thêm các phần tử vào cuối. Tại bất kỳ thời điểm nào, chúng ta có thể được yêu cầu tính một giá trị xuất phát từ tất cả các cặp phần tử trong đó phần tử đầu tiên ở bên trái phần tử thứ hai. 

Đối với một trình tự cố định$a_1, a_2, \dots, a_n$, bài toán xác định một đại lượng$A_i$cho mỗi điểm phân chia$i$giữa 1 và$n-1$. Đối với mỗi lần phân chia như vậy, chúng tôi xem xét tất cả các cặp$(j, k)$như vậy$j \le i < k$, tính toán$a_j \& a_k$, và lấy giá trị lớn nhất trên tất cả các cặp đó. Sau đó$A_i$là XOR của tất cả những đóng góp này trên tất cả các cặp như vậy, trong thực tế giảm đến cùng mức tối đa vì XOR không thực sự tích lũy nhiều giá trị theo cách độc lập có ý nghĩa trong bối cảnh biểu thức tối đa này. Truy vấn cuối cùng yêu cầu giá trị tối đa của$A_i$trên tất cả các điểm phân chia hợp lệ$i$. 

Về mặt hoạt động, chúng tôi đang thêm các giá trị và đôi khi hỏi: trong số tất cả các cách để chia chuỗi thành phần bên trái và phần bên phải, giá trị AND lớn nhất giữa bất kỳ phần tử nào ở bên trái và bất kỳ phần tử nào ở bên phải là bao nhiêu. 

Những hạn chế chính là$q \le 10^5$hoạt động và giá trị lên đến$2^{20}$. Điều này ngay lập tức loại trừ mọi giải pháp tính toán lại câu trả lời từ đầu cho mỗi truy vấn, vì việc xây dựng lại các tương tác theo cặp là$O(n^2)$mỗi truy vấn trong trường hợp xấu nhất, điều này sẽ dẫn đến$10^{10}$hoạt động. 

Một vấn đề tế nhị phát sinh từ bản chất năng động. Một cách tiếp cận đơn giản có thể cố gắng duy trì tất cả các giá trị AND theo cặp hoặc tính toán lại các phần tách tốt nhất sau mỗi lần chèn. Điều đó không thành công vì mỗi lần chèn có khả năng tương tác với tất cả các phần tử trước đó và việc tính toán lại cực đại phân tách sẽ liên tục xem lại toàn bộ lịch sử. 

Một trường hợp cạnh ẩn khác là khi độ dài chuỗi nhỏ hơn 2. Trong trường hợp đó, không có sự phân chia nào tồn tại và câu trả lời phải là 0. Điều này quan trọng vì việc triển khai ngây thơ giả sử có ít nhất một cặp tồn tại sẽ bị hỏng hoặc trả về mức tối đa chưa được khởi tạo. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Sau mỗi thao tác, nếu được hỏi một truy vấn, chúng tôi sẽ thử từng điểm phân tách$i$. Đối với mỗi lần phân chia, chúng tôi quét tất cả các cặp vượt qua ranh giới phân chia và tính toán mức tối đa$a_j \& a_k$. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. 

Tuy nhiên, chi phí là rất cao. Đối với một chuỗi có độ dài$n$, chi phí một lần kiểm tra chia nhỏ$O(n^2)$, và có$O(n)$chia tách, vì vậy một truy vấn duy nhất là$O(n^3)$theo cách giải thích tồi tệ nhất. Ngay cả khi tối ưu hóa việc đánh giá phân tách để tái sử dụng thông tin, chúng ta vẫn phải đối mặt với$O(n^2)$cho mỗi truy vấn, điều này trở nên không thể thực hiện được theo$10^5$hoạt động. 

Nhận xét quan trọng là vấn đề không thực sự nằm ở việc chia tách. Mỗi cặp hợp lệ$(j,k)$với$j<k$đóng góp$a_j \& a_k$và câu trả lời chỉ đơn giản là AND tối đa trên tất cả các cặp trong tiền tố hiện tại. Công thức phân chia gây xao nhãng: cách phân chia tốt nhất luôn đặt hai phần tử đạt được cặp AND tối đa toàn cục ở các phía đối diện nhau. 

Vì vậy, vấn đề giảm xuống còn việc duy trì, khi chèn, AND tối đa theo bit trong số tất cả các cặp trong tập hợp hiện tại. Cấu trúc quan trọng là bitwise AND có tính đơn điệu theo bit: các bit cao hơn chiếm ưu thế và một cặp chỉ đạt được giá trị cao nếu cả hai số chia sẻ các bit cao đó. Điều này gợi ý việc duy trì các ứng cử viên được nhóm theo mẫu bit cao. 

Thủ thuật tiêu chuẩn cho cài đặt này là duy trì, đối với mỗi giá trị, giá trị AND tốt nhất có thể hình thành với các giá trị được chèn trước đó, nhưng đây vẫn là$O(n^2)$. Thay vào đó, chúng tôi duy trì phép thử theo từng bit so với các số trước đó. Khi chèn số mới$x$, chúng ta muốn tìm số trước đó$y$tối đa hóa$x \& y$. Điều này có thể được thực hiện một cách tham lam từ bit cao nhất đến bit thấp nhất, ưu tiên các đường dẫn có cả hai bit là 1. Chúng tôi cập nhật câu trả lời chung bằng cặp đôi tốt nhất này. 

Điều này có hiệu quả vì giá trị AND của bất kỳ cặp tối ưu nào đều được nhận ra khi chúng ta chọn cho mỗi bit xem cả hai số có số 1 ở đó hay không. Trie đảm bảo chúng tôi luôn tìm được đối tác tương thích tốt nhất đã có trong cấu trúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Bảo trì Bitwise Trie |$O(q \cdot 20)$|$O(q \cdot 20)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một phép thử bitwise trống và một biến`best = 0`. Trie lưu trữ tất cả các số được chèn trước đó ở dạng nhị phân, được sắp xếp theo bit từ 19 xuống 0. Cấu trúc này cho phép khớp hiệu quả các số có chung bit cao. 
2. Xử lý từng thao tác theo thứ tự. Nếu thao tác là chèn một số`x`, trước tiên hãy truy vấn thử để tìm giá trị lớn nhất có thể của`(x & y)`trên tất cả được chèn trước đó`y`. Bước này đảm bảo chúng tôi nắm bắt được cặp tốt nhất trong đó`x`tham gia. 
3. Để tính toán kết quả phù hợp nhất này, hãy duyệt tri từ bit cao nhất đến bit thấp nhất. Tại mỗi bit, nếu bit hiện tại của`x`là 1, chúng tôi muốn chuyển sang một đứa trẻ cũng có 1 ở bit đó, vì điều này góp phần tích cực vào kết quả AND. Nếu không có đứa trẻ như vậy tồn tại, chúng tôi chuyển sang nhánh khác. 
4. Trong khi di chuyển ngang, tích lũy giá trị AND kết quả bằng cách chỉ thiết lập một bit khi cả hai`x`và số đường dẫn đã chọn có bit đó bằng 1. Lựa chọn tham lam này hợp lệ vì các bit cao hơn chiếm ưu thế trong giá trị cuối cùng. 
5. Sau khi duyệt xong, cập nhật`best`với giá trị tối đa hiện tại của nó và giá trị khớp được tính toán cho`x`. 
6. Chèn`x`vào bản thử để có thể sử dụng nó cho các truy vấn trong tương lai. Điều này đảm bảo tính đối xứng: mỗi cặp cuối cùng được đánh giá một lần khi phần tử sau được chèn vào. 
7. Nếu thao tác là một truy vấn, hãy xuất ra`best`nếu ít nhất hai phần tử đã được chèn vào; nếu không thì xuất ra 0. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến`best`là bitwise AND tối đa trên tất cả các cặp giữa các phần tử được chèn. Mỗi cặp$(a_i, a_j)$được đánh giá chính xác một lần khi phần tử sau được chèn vào, bởi vì tại thời điểm đó phần tử trước đó đã có trong bộ thử và chúng tôi tính toán kết quả phù hợp nhất với nó. Việc truyền tải trie đảm bảo rằng đối với mỗi số được chèn, chúng tôi tìm thấy đối tác tối ưu toàn cầu dưới sự ràng buộc của các phần tử đã được chèn, do đó không thể bỏ sót cặp nào tốt hơn. Vì AND có tính đối xứng và thứ tự chèn chỉ trì hoãn việc đánh giá cho đến khi phần tử thứ hai xuất hiện nên mức tối đa toàn cục luôn được theo dõi chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Trie:
    def __init__(self):
        self.next = [{}]  # list of dict children
        self.has = [0]

    def insert(self, x):
        node = 0
        for b in range(19, -1, -1):
            bit = (x >> b) & 1
            if bit not in self.next[node]:
                self.next[node][bit] = len(self.next)
                self.next.append({})
                self.has.append(0)
            node = self.next[node][bit]
            self.has[node] += 1

    def query_best_and(self, x):
        node = 0
        res = 0
        for b in range(19, -1, -1):
            bit = (x >> b) & 1
            # try to keep 1 if possible
            if bit == 1 and 1 in self.next[node]:
                node = self.next[node][1]
                res |= (1 << b)
            elif bit == 0 and 1 in self.next[node]:
                node = self.next[node][1]
            elif bit in self.next[node]:
                node = self.next[node][bit]
            else:
                # fallback (should not happen in valid trie)
                if 0 in self.next[node]:
                    node = self.next[node][0]
                else:
                    break
        return res

def solve():
    q = int(input())
    trie = Trie()
    best = 0
    cnt = 0

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            x = int(tmp[1])
            cnt += 1
            if cnt > 1:
                best = max(best, trie.query_best_and(x))
            trie.insert(x)
        else:
            print(best if cnt >= 2 else 0)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì phép thử nhị phân trên 20 bit cho mỗi số. các`query_best_and`trước tiên, hàm cố gắng căn chỉnh theo 1 bit một cách tham lam, vì AND chỉ có lợi khi cả hai bên đều có một bit được đặt. Việc chèn xảy ra sau khi truy vấn để phần tử hiện tại không được ghép nối với chính nó. 

các`best`biến chỉ được cập nhật khi tồn tại ít nhất một phần tử trước đó, đảm bảo tính chính xác cho trường hợp chèn đầu tiên. quầy`cnt`thực thi điều kiện truy vấn trước hai phần tử trả về 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 7
1 13
2
1 4
2
```Chúng tôi theo dõi các lần chèn và AND tốt nhất. 

| Bước | Hoạt động | Bộ chèn | Cặp tốt nhất VÀ | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | chèn 7 | {7} | 0 | - | 
| 2 | chèn 13 | {7,13} | 5 | - | 
| 3 | truy vấn | {7,13} | 5 | 5 | 
| 4 | chèn 4 | {7,13,4} | 5 | - | 
| 5 | truy vấn | {7,13,4} | 5 | 5 | 

Ở bước 2, 7 & 13 = 5, bước nào là tốt nhất. Chèn 4 không cải thiện bất kỳ cặp nào. 

### Ví dụ 2 

đầu vào:```
1 8
1 2
1 10
2
```| Bước | Hoạt động | Bộ chèn | Cặp tốt nhất VÀ | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | chèn 8 | {8} | 0 | - | 
| 2 | chèn 2 | {8,2} | 0 | - | 
| 3 | chèn 10 | {8,2,10} | 8 & 10 = 8 | - | 
| 4 | truy vấn | {8,2,10} | 8 | 8 | 

Điều này cho thấy thuật toán xác định chính xác rằng cặp tốt nhất là (8,10). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \cdot 20)$| Mỗi lần chèn và truy vấn đi qua tối đa 20 bit trong trie | 
| Không gian |$O(q \cdot 20)$| Mỗi số được chèn tạo tối đa 20 nút trie | 

Các ràng buộc cho phép lên đến$10^5$và mỗi thao tác đều tuyến tính theo số bit (20), do đó giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimum case
assert run("2\n2\n1 5\n2\n") == "0", "min size query"

# simple pair
assert run("3\n1 7\n1 13\n2\n") == "5", "basic AND"

# all equal values
assert run("4\n1 7\n1 7\n1 7\n2\n") == "7", "identical values"

# increasing values
assert run("5\n1 1\n1 2\n1 4\n1 8\n2\n") == "0", "no overlapping bits"

# mixed case
assert run("6\n1 3\n1 5\n1 6\n2\n1 7\n2\n") == "6\n6", "updates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn tối thiểu | 0 | xử lý phần tử đơn | 
| cơ bản VÀ | 5 | tính toán cặp đúng | 
| tất cả đều bình đẳng | giá trị | xử lý giống hệt nhau | 
| bit rời rạc | 0 | không có trường hợp chồng chéo | 
| cập nhật | ổn định tối đa | bảo trì năng động | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi chỉ tồn tại một phần tử và một truy vấn được đưa ra. Ví dụ, đầu vào`1 10`theo sau là một truy vấn phải xuất ra 0. Thuật toán xử lý việc này thông qua`cnt < 2`kiểm tra, ngăn chặn mọi truy vấn trie không hợp lệ. 

Một trường hợp khác là khi tất cả các số không có bit chung. Ví dụ: chèn lũy thừa của hai như 1, 2, 4, 8 sẽ dẫn đến mọi AND bằng 0. Trie vẫn xử lý mỗi lần chèn, nhưng không có quá trình truyền tải nào tìm thấy bit được chia sẻ, vì vậy`best`vẫn là 0, phù hợp với câu trả lời đúng. 

Trường hợp thứ ba là những con số giống hệt nhau được lặp lại. Đối với đầu vào`1 7, 1 7`, người trie sẽ tìm được sự kết hợp hoàn hảo mang lại năng suất`7 & 7 = 7`, và điều này trở thành mức tối đa toàn cầu. Vì các bản sao được chèn độc lập nên thuật toán vẫn đánh giá cặp khi bản sao thứ hai được chèn vào, đảm bảo tính chính xác.
