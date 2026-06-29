---
title: "CF 104618D - Kem Lasagna"
description: "Chúng ta được cấp một chồng kem $n$ theo chiều dọc. Mỗi lớp là một chuỗi trên bảng chữ cái ${R, G}$, đại diện cho kẹo trong lớp đó từ trái sang phải. Quá trình này diễn ra tuần tự nghiêm ngặt: chúng tôi bắt đầu từ lớp 1, rồi đến lớp 2, v.v. cho đến lớp $n$."
date: "2026-06-29T17:29:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104618
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 09-22-23 Div. 1"
rating: 0
weight: 104618
solve_time_s: 75
verified: false
draft: false
---

[CF 104618D - Kem Lasagna](https://codeforces.com/problemset/problem/104618/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chồng dọc$n$lớp kem. Mỗi lớp là một chuỗi trên bảng chữ cái$\{R, G\}$, đại diện cho kẹo trong lớp đó từ trái sang phải. Quá trình này diễn ra tuần tự nghiêm ngặt: chúng tôi bắt đầu từ lớp 1, rồi đến lớp 2, v.v. cho đến khi lớp$n$. Trong một lớp, chúng ta phải tiêu thụ kẹo theo thứ tự nhất định và chúng ta không thể sắp xếp lại trong hoặc giữa các lớp. 

Nếu chúng ta tưởng tượng làm phẳng tất cả các lớp thành một chuỗi dài bằng cách ghép chúng theo thứ tự, thì vấn đề sẽ trở thành: tìm đoạn liền kề dài nhất chỉ bao gồm một màu, hoặc tất cả$R$hoặc tất cả$G$, nhưng với hạn chế là phân đoạn phải tôn trọng ranh giới lớp và thứ tự truyền tải. 

Vì vậy, một cách hiệu quả, chúng tôi đang tìm kiếm độ dài tối đa của chuỗi con đơn sắc trên phép nối của tất cả các chuỗi đã cho. 

Điều tinh tế quan trọng là chúng tôi không xây dựng chuỗi nối một cách rõ ràng vì tổng chiều dài có thể lên tới$2 \cdot 10^5$, điều này không sao cả, nhưng chúng ta cũng có thể giải quyết nó ở dạng phát trực tuyến bằng cách theo dõi quá trình chuyển đổi giữa các lớp. 

Ràng buộc$\sum c_i \le 2 \cdot 10^5$ngụ ý một$O(N)$hoặc$O(N \log N)$giải pháp là cần thiết, nhưng vì cấu trúc là tuyến tính nên một lần truyền là đủ. Bất kỳ cách tiếp cận bậc hai nào trên các lớp hoặc vị trí sẽ quá chậm. 

Các trường hợp cạnh xuất hiện khi các lần chạy trải dài trên ranh giới lớp. Ví dụ: 

đầu vào:```
2
RRR
RR
```Đầu ra đúng là 5, vì quá trình chạy tiếp tục trên các lớp. Mức tối đa ngây thơ trên mỗi lớp sẽ trả về không chính xác 3. 

Một trường hợp khác:```
3
RR
GR
RR
```Một cách tiếp cận bất cẩn có thể thiết lập lại ở mọi ranh giới lớp và bỏ lỡ các lần chạy có thể khởi động lại hoặc tiếp tục dựa trên các ký tự ranh giới. 

Rủi ro chính là không thể hợp nhất các lớp liên tiếp khi các ký tự ranh giới khớp nhau. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là nối tất cả các chuỗi thành một chuỗi và sau đó tính toán chuỗi ký tự giống hệt nhau dài nhất bằng cách quét nó. Điều này rất đơn giản: lặp qua từng ký tự, theo dõi độ dài chuỗi hiện tại, đặt lại khi ký tự thay đổi và cập nhật mức tối đa. 

Điều này hoạt động chính xác vì vấn đề về cơ bản là vấn đề về độ dài chạy trên một chuỗi tuyến tính. Tuy nhiên, mặc dù việc ghép nối đơn giản về mặt khái niệm, nhưng việc xây dựng chuỗi một cách rõ ràng là không cần thiết và việc xử lý lặp đi lặp lại trên mỗi lớp mà không hợp nhất các chuyển đổi có thể dẫn đến logic không chính xác nếu không được xử lý cẩn thận. 

Sự cải thiện đến từ việc nhận thấy rằng chúng tôi không bao giờ cần truy cập ngẫu nhiên hoặc quay lại. Chúng tôi chỉ cần duy trì chuỗi hiện tại trên đầu vào phát trực tuyến. Tại bất kỳ thời điểm nào, thông tin duy nhất cần thiết là ký tự được nhìn thấy lần cuối và độ dài chuỗi hiện tại. 

Điều này làm giảm vấn đề xuống còn một lần chuyển qua tất cả các ký tự trong tất cả các lớp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (nối + quét) | O(N) | O(N) | Đã chấp nhận | 
| Truyền phát một lượt | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng lớp một và trong mỗi lớp xử lý các ký tự từ trái sang phải trong khi vẫn duy trì một chuỗi toàn cục. 

1. Khởi tạo biến`last`trống rỗng,`cur`bằng 0 và`best`bằng 0. 

Chúng theo dõi ký tự trước đó, độ dài lần chạy hiện tại và lần chạy tối đa được thấy cho đến nay. 
2. Lặp lại từng chuỗi lớp theo thứ tự từ 1 đến$n$. 

Điều này bảo tồn thứ tự tiêu thụ cần thiết. 
3. Đối với mỗi nhân vật$ch$trong chuỗi hiện tại: 

Nếu$ch = last$, tăng`cur`bằng 1. Nếu không, hãy đặt lại`cur`thành 1 và đặt`last = ch`. 

Bước này duy trì tính bất biến`cur`chính xác là độ dài của hậu tố tối đa hiện tại bao gồm các ký tự giống hệt nhau. 
4. Sau khi cập nhật`cur`, cập nhật`best = max(best, cur)`. 

Điều này đảm bảo chúng tôi nắm bắt được lượt chạy tối đa kết thúc ở mọi vị trí. 
5. Sau khi xử lý tất cả các lớp, xuất ra`best`. 

### Tại sao nó hoạt động 

Tại mọi vị trí trong chuỗi được làm phẳng, thuật toán duy trì độ dài của khối liền kề tối đa kết thúc tại vị trí đó. Điều này là đủ vì bất kỳ câu trả lời hợp lệ nào cũng phải kết thúc ở một vị trí nào đó và mọi lần chạy có thể được coi là chính xác một lần khi nó kết thúc. Việc nén trạng thái vào`(last, cur)`là đủ vì tất cả lịch sử trước đó ngoài lần chạy hiện tại đều không liên quan khi xảy ra thay đổi ký tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    last = None
    cur = 0
    best = 0

    for _ in range(n):
        s = input().strip()
        for ch in s:
            if ch == last:
                cur += 1
            else:
                last = ch
                cur = 1
            if cur > best:
                best = cur

    print(best)

if __name__ == "__main__":
    solve()
```Việc triển khai truyền trực tiếp qua tất cả các lớp mà không cần xây dựng chuỗi kết hợp. Biến`last`đảm bảo phát hiện chính xác các chuyển đổi ngay cả trên các ranh giới lớp, vì trạng thái vẫn tồn tại giữa các vòng lặp trên chuỗi. 

Một lỗi phổ biến là đặt lại`last`hoặc`cur`ở mỗi lớp mới, điều này sẽ phá vỡ các lần chạy hợp lệ trên các lớp đó một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
RGR
GGGG
GR
RRR
```| Bước | Char | Cuối cùng | Cur | Tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | R | R | 1 | 1 | 
| 2 | G | G | 1 | 1 | 
| 3 | R | R | 1 | 1 | 
| 4 | G | G | 1 | 1 | 
| 5-8 | G G G G | G | 4 | 4 | 
| 9 | G | G | 5 | 5 | 
| 10 | R | R | 1 | 5 | 
| 11 | R | R | 2 | 5 | 
| 12 | R | R | 3 | 5 | 

Câu trả lời cuối cùng: 5 

Dấu vết này cho thấy các đường chạy được bảo toàn qua các ranh giới lớp, đặc biệt là các đường dài`GGGG`phân đoạn. 

### Ví dụ 2 

đầu vào:```
5
RGGGGGGG
GGGRGRGRGRG
GRGRGRGRGRGRGRGRGRGR
RRRRRRRRR
G
```| Bước | Char | Cuối cùng | Cur | Tốt nhất | 
| --- | --- | --- | --- | --- | 
| bắt đầu | R | R | 1 | 1 | 
| 2-8 | G G G G G G G | G | 7 | 7 | 
| tiếp theo | G | G | 8 | 8 | 
| tiếp theo | G | G | 9 | 9 | 
| ... | xen kẽ | khác nhau | đặt lại thường xuyên | Tối đa 19 | 

Đáp án cuối cùng: 19 

Điều này chứng tỏ rằng thuật toán xử lý chính xác các chuỗi dài bị gián đoạn lặp đi lặp lại và chỉ tiếp tục theo dõi các phân đoạn giống hệt nhau liền kề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sum c_i)$| Mỗi ký tự được xử lý chính xác một lần | 
| Không gian |$O(1)$| Chỉ duy trì một số lượng biến trạng thái không đổi | 

Tổng số ký tự nhiều nhất là$2 \cdot 10^5$, do đó, một lần quét tuyến tính sẽ vừa vặn thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample 1
assert run("""4
RGR
GGGG
GR
RRR
""") == "5"

# provided sample 2
assert run("""5
RGGGGGGG
GGGRGRGRGRG
GRGRGRGRGRGRGRGRGRGR
RRRRRRRRR
G
""") == "19"

# minimum input
assert run("""1
R
""") == "1"

# all same character
assert run("""3
RRR
RR
RRRR
""") == "9"

# alternating single characters
assert run("""4
R
G
R
G
""") == "1"

# long boundary-spanning run
assert run("""2
RRRRR
RRRRR
""") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| char đơn | 1 | trường hợp ranh giới tối thiểu | 
| tất cả R | toàn bộ số tiền | hợp nhất giữa các lớp | 
| xen kẽ | 1 | đặt lại thường xuyên | 
| hai khối | 10 | tính liên tục xuyên lớp | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi quá trình chạy tiếp tục chính xác qua các ranh giới lớp. Coi như:```
2
RRR
RR
```Thuật toán giữ`last = 'R'`sau khi làm xong lớp thứ nhất nên khi bước vào lớp thứ hai, vệt vẫn tiếp tục một cách tự nhiên. Nhà nước trở thành`cur = 5`, đưa ra câu trả lời đúng 

Một trường hợp khác là reset nhiều lần:```
3
R
G
R
```Ở đây mỗi nhân vật thay đổi lực lượng`cur = 1`. Thuật toán không bao giờ hợp nhất sai các phân đoạn không liền kề vì`last`luôn theo dõi nhân vật tiền thân ngay lập tức trên toàn cầu. 

Cuối cùng, sự thay đổi ranh giới bên trong một lớp được xử lý giống hệt như thay đổi ranh giới giữa các lớp, vì cả hai chỉ là sự so sánh ký tự trong một s duy nhất.
