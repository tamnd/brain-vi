---
title: "CF 1566B - Cắt MIN-MEX"
description: "Chúng ta được cấp một chuỗi nhị phân và có thể chia nó thành bất kỳ số phần liền kề nào. Mỗi nhân vật phải thuộc về chính xác một mảnh. Đối với mỗi phần, chúng tôi tính toán MEX của nó."
date: "2026-06-10T11:56:24+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1566
codeforces_index: "B"
codeforces_contest_name: "Codeforces Global Round 16"
rating: 800
weight: 1566
solve_time_s: 308
verified: false
draft: false
---

[CF 1566B - Cắt MIN-MEX](https://codeforces.com/problemset/problem/1566/B) 

**Đánh giá:** 800 
**Tags:** bitmasks, thuật toán xây dựng, dp, tham lam 
**Thời gian giải:** 5 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi nhị phân và có thể chia nó thành bất kỳ số phần liền kề nào. Mỗi nhân vật phải thuộc về chính xác một mảnh. 

Đối với mỗi phần, chúng tôi tính toán MEX của nó. Vì chuỗi chỉ chứa`0`Và`1`, chỉ có ba giá trị có thể có: 

| Nội dung mảnh | MEX | 
| --- | --- | 
| Không chứa`0`| 0 | 
| Chứa`0`nhưng không`1`| 1 | 
| Chứa cả hai`0`Và`1`| 2 | 

Mục tiêu của chúng tôi là chọn các phần cắt sao cho tổng giá trị MEX của tất cả các phần càng nhỏ càng tốt. 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là$10^5$. Điều đó có nghĩa là chúng ta nên mong đợi một nghiệm gần với thời gian tuyến tính. Phương pháp bậc hai sẽ yêu cầu khoảng$10^{10}$trong trường hợp xấu nhất vượt xa giới hạn. 

Phần khó khăn là hiểu khi nào việc cắt giúp ích và khi nào nó gây đau. 

Hãy xem xét chuỗi`1111`. Toàn bộ chuỗi chỉ chứa`1`s, vậy MEX của nó là`0`. Câu trả lời là`0`. Một giải pháp luôn cố gắng tạo ra nhiều phần hơn sẽ bỏ lỡ điều này. 

Hãy xem xét chuỗi`0000`. Toàn bộ chuỗi chỉ chứa`0`s, vậy MEX của nó là`1`. Câu trả lời là`1`. Việc chia nó thành nhiều phần hoàn toàn bằng 0 sẽ mang lại cho MEX`1`cho mỗi phần và chỉ làm tăng tổng. 

Hãy xem xét chuỗi`01010`. Toàn bộ chuỗi chứa cả hai chữ số, vì vậy MEX của nó là`2`. Một quan sát bất cẩn có thể gợi ý rằng mọi chuỗi chứa cả hai chữ số đều có câu trả lời`2`. Điều đó là sai. Chúng ta có thể cắt nó như`0 | 1010`, cho`1 + 1 = 2`, nhưng chúng tôi không thể làm tốt hơn. 

Bây giờ hãy xem xét`100001`. Toàn bộ chuỗi có MEX`2`, nhưng cắt như`1 | 0000 | 1`cho`0 + 1 + 0 = 1`. Câu trả lời trở thành`1`. Hiểu lý do tại sao điều này xảy ra là cái nhìn sâu sắc chính của vấn đề. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ thử mọi tập hợp vị trí cắt có thể. Một chuỗi có độ dài$n$có$n-1$các vị trí cắt tiềm năng và mỗi vị trí có thể có hoặc không có vết cắt. Điều đó mang lại$2^{n-1}$phân vùng. 

Đối với mỗi phân vùng, chúng ta có thể tính MEX của mỗi phần và giữ số tiền tối thiểu. Điều này đúng vì nó kiểm tra mọi câu trả lời hợp lệ, nhưng nó trở nên không thể ngay cả đối với$n=50$, huống hồ là$10^5$. 

Để tìm ra một mô hình, chúng ta hãy xem xét yếu tố nào đóng góp vào chi phí dương. 

Bất kỳ phần nào chỉ chứa`1`s có MEX`0`, vì vậy những phần như vậy là miễn phí. 

Bất kỳ mảnh nào có chứa ít nhất một`0`chi phí ít nhất`1`. 

Điều này ngay lập tức gợi ý rằng ký tự duy nhất mà chúng ta thực sự quan tâm là số không. 

Giả sử tất cả các số 0 thuộc về một khối liền kề, ví dụ`111000111`. Chúng ta có thể cô lập khối 0 và bao quanh nó bằng các phần nguyên khối:`111 | 000 | 111`Chi phí là`0 + 1 + 0 = 1`. 

Bây giờ giả sử chuỗi có hai khối 0 riêng biệt, ví dụ`0011100`. 

Hai nhóm số 0 không thể hợp nhất thành một phần hoàn toàn bằng 0 vì có những nhóm ở giữa chúng. Chúng ta có hai sự lựa chọn. 

Chúng ta có thể giữ mọi thứ lại với nhau, tạo ra một mảnh chứa cả hai chữ số và thanh toán`2`. 

Hoặc chúng ta có thể cô lập cả hai nhóm 0:`00 | 111 | 00`chi phí nào`1 + 0 + 1 = 2`. 

Dù sao thì câu trả lời là`2`. 

Quan sát này mô tả đầy đủ vấn đề. 

Nếu không có số 0 nào cả, câu trả lời là`0`. 

Nếu có chính xác một khối số 0 liền kề nhau thì câu trả lời là`1`. 

Nếu có hai hoặc nhiều khối 0, câu trả lời là`2`. 

Không có gì lớn hơn`2`bao giờ cũng tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi nhị phân. 
2. Đếm xem có bao nhiêu khối liền kề nhau`0`xuất hiện trong chuỗi. 
3. Nếu số khối bằng 0 là`0`, đầu ra`0`. 

Chuỗi bao gồm hoàn toàn`1`s, vậy toàn bộ chuỗi có MEX`0`. 
4. Nếu số khối bằng 0 là`1`, đầu ra`1`. 

Chúng ta có thể tách khối 0 đó thành một khối chỉ chứa các số 0. Tất cả các phần còn lại chỉ chứa những phần duy nhất và đóng góp chi phí bằng không. 
5. Nếu số khối bằng 0 ít nhất`2`, đầu ra`2`. 

Có ít nhất hai nhóm 0 riêng biệt tồn tại. Chúng không thể được hợp nhất thành một mảnh hoàn toàn bằng không vì chúng tách rời chúng. Chi phí tốt nhất có thể chính xác là`2`. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là mọi phần chứa số 0 đều đóng góp ít nhất`1`để trả lời. 

Nếu chuỗi chứa một khối 0, tất cả các số 0 có thể được đặt bên trong một phần hoàn toàn bằng 0, tạo ra tổng chi phí`1`. 

Nếu chuỗi chứa nhiều khối 0, chúng ta có hai khả năng. Chúng ta có thể giữ một số số cùng với số 0, tạo ra một mảnh có MEX`2`, hoặc cô lập các khối 0 và trả tiền`1`cho từng phần có liên quan. Chiến lược nào cũng đạt được chi phí`2`và không có chiến lược nào có thể đạt được chi phí`1`bởi vì một mảnh hoàn toàn bằng 0 không thể bao phủ nhiều khối 0 riêng biệt. 

Do đó, câu trả lời chỉ phụ thuộc vào số khối 0:$$\min(2,\text{number of zero blocks})$$với trường hợp đặc biệt là khối 0 bằng 0 sẽ tạo ra câu trả lời`0`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        s = input().strip()

        blocks = 0
        n = len(s)

        for i in range(n):
            if s[i] == '0' and (i == 0 or s[i - 1] == '1'):
                blocks += 1

        ans.append(str(min(blocks, 2)))

    sys.stdout.write("\n".join(ans))

solve()
```Việc triển khai chỉ tính các phân đoạn 0 liền kề. 

Khối 0 mới bắt đầu bất cứ khi nào chúng ta thấy`0`có ký tự trước đó vắng mặt hoặc bằng`1`. Điều kiện đó xác định ký tự đầu tiên của mỗi lần chạy số 0. 

Khi đã biết số khối bằng 0, câu trả lời đơn giản là`min(blocks, 2)`. Công thức nhỏ gọn này bao gồm tất cả các trường hợp một cách tự nhiên: 

Nếu`blocks = 0`, kết quả là`0`. 

Nếu như`blocks = 1`, kết quả là`1`. 

Nếu như`blocks >= 2`, kết quả là`2`. 

Không cần xử lý bổ sung. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào:`01100`| Chỉ mục | Nhân vật | Khối số 0 mới? | Khối | 
| --- | --- | --- | --- | 
| 0 | 0 | Có | 1 | 
| 1 | 1 | Không | 1 | 
| 2 | 1 | Không | 1 | 
| 3 | 0 | Có | 2 | 
| 4 | 0 | Không | 2 | 

Giá trị cuối cùng:`blocks = 2`Trả lời:`min(2, 2) = 2`Ví dụ này cho thấy hai nhóm 0 riêng biệt. Cho dù chúng ta cắt dây như thế nào thì chi phí tối thiểu có thể đạt được là`2`. 

### Ví dụ 2 

Chuỗi đầu vào:`100001`| Chỉ mục | Nhân vật | Khối số 0 mới? | Khối | 
| --- | --- | --- | --- | 
| 0 | 1 | Không | 0 | 
| 1 | 0 | Có | 1 | 
| 2 | 0 | Không | 1 | 
| 3 | 0 | Không | 1 | 
| 4 | 0 | Không | 1 | 
| 5 | 1 | Không | 1 | 

Giá trị cuối cùng:`blocks = 1`Trả lời:`min(1, 2) = 1`Các số không tạo thành một phân đoạn liền kề. Cắt như`1 | 0000 | 1`đạt được chi phí`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi ký tự được kiểm tra một lần | 
| Không gian |$O(1)$| Chỉ có một vài quầy được lưu trữ | 

Vì tổng độ dài đầu vào của tất cả các trường hợp thử nghiệm nhiều nhất là$10^5$, một lần quét tuyến tính trên mỗi chuỗi dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ không đổi bất kể độ dài chuỗi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        s = input().strip()

        blocks = 0
        for i, ch in enumerate(s):
            if ch == '0' and (i == 0 or s[i - 1] == '1'):
                blocks += 1
```
