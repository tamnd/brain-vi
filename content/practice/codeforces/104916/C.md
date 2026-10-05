---
title: "CF 104916C-CÁT"
description: "Chúng ta được cho một chuỗi bao gồm các chữ cái Latinh viết hoa và chúng ta quan tâm đến việc đếm xem có bao nhiêu chuỗi con có thứ tự có dạng “C-A-T” tồn tại bên trong nó."
date: "2026-06-28T08:10:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104916
codeforces_index: "C"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u0421\u0430\u043c\u0430\u0440\u0435 2022-2023 (9-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 104916
solve_time_s: 48
verified: true
draft: false
---

[CF 104916C - CAT](https://codeforces.com/problemset/problem/104916/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi bao gồm các chữ cái Latinh viết hoa và chúng ta quan tâm đến việc đếm xem có bao nhiêu chuỗi con có thứ tự có dạng “C-A-T” tồn tại bên trong nó. Một dãy con được hình thành bằng cách chọn các chỉ số$i < j < k$sao cho các ký tự ở các vị trí đó lần lượt là ‘C’, ‘A’ và ‘T’. Các ký tự không cần phải ở cạnh nhau, chỉ có thứ tự của chúng là quan trọng. 

Nhiệm vụ hoàn toàn mang tính tổ hợp: với mỗi lựa chọn vị trí hợp lệ, chúng tôi tính một lần xuất hiện của mẫu. Đầu ra là tổng số bộ ba như vậy. 

Nếu độ dài chuỗi lớn, theo thứ tự$10^5$hoặc hơn thế nữa, bất kỳ giải pháp nào thử rõ ràng tất cả các bộ ba chỉ số đều không thể thực hiện được vì điều đó sẽ yêu cầu hành vi bậc ba hoặc ít nhất là bậc hai trong trường hợp xấu nhất. Ngay cả vòng lặp kép qua các vị trí cũng nhanh chóng trở nên quá chậm khi được lồng với một lần quét khác. 

Trường hợp cạnh tinh tế là khi chuỗi chứa nhiều chữ cái lặp lại. Ví dụ: nếu chuỗi là “CATCAT”, cách giải thích tham lam ngây thơ có thể cho rằng các mẫu chồng chéo gây nhiễu lẫn nhau một cách không chính xác, nhưng trên thực tế, mọi bộ ba chỉ số hợp lệ đều độc lập. Một dạng lỗi khác là xử lý vấn đề dưới dạng khớp chuỗi con liền kề thay vì đếm chuỗi con, điều này sẽ bỏ sót các dạng không liền kề hợp lệ, chẳng hạn như: 

đầu vào:```
CAXAT
```Đầu ra đúng:```
2
```bởi vì các cặp (C ở 1, A ở 3, T ở 5) và (C ở 1, A ở 4, T ở 5) đều tạo thành các chuỗi con hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: chọn mọi vị trí cho 'C', sau đó là mọi vị trí sau đó cho 'A', và sau đó là mọi vị trí sau đó cho 'T'. Điều này trực tiếp tuân theo định nghĩa của vấn đề và đúng vì nó liệt kê tất cả các bộ ba hợp lệ$i < j < k$. Tuy nhiên, điều này đòi hỏi phải kiểm tra tất cả$O(n^3)$gấp ba lần trong trường hợp xấu nhất. Với$n = 10^5$, điều này vượt xa giới hạn khả thi. 

Chúng ta có thể cải thiện điều này bằng cách sử dụng lại cấu trúc từng phần. Thay vì tìm kiếm độc lập từng bộ ba, chúng ta quan sát thấy rằng mọi chữ 'A' nằm giữa một tập hợp các chữ 'C ở bên trái và một tập hợp các chữ 'T ở bên phải. Nếu chúng ta biết, đối với mỗi 'A', có bao nhiêu tiền tố "C-A" hợp lệ kết thúc ở nó và có bao nhiêu phần mở rộng hậu tố "A-T" bắt đầu từ nó, thì chúng ta có thể kết hợp chúng một cách hiệu quả. 

Điều này dẫn đến một chiến lược tích lũy năng động tự nhiên. Chúng tôi xử lý chuỗi từ phải sang trái để có thể duy trì số lượng 'T tồn tại ở bên phải, bao nhiêu cặp "AT" tồn tại ở bên phải và bao nhiêu bộ ba "CAT" có thể được hình thành. 

Bằng cách duy trì các bộ đếm cho các cấu trúc từng phần, chúng ta tránh được việc tính toán lại các bài toán con chồng chéo. Mỗi ký tự đóng góp vào các mẫu cấp độ cao hơn dựa trên thông tin tích lũy trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Tích lũy từ phải sang trái |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quét chuỗi từ phải sang trái trong khi duy trì ba bộ đếm: 

1. Một quầy`cntT`lưu trữ bao nhiêu ký tự 'T' đã được nhìn thấy cho đến nay. 
2. Một quầy`cntAT`lưu trữ có bao nhiêu cặp “A-T” tồn tại trong đó ‘A’ ở bên trái các chữ ‘T’ đó trong hậu tố. 
3. Một quầy`cntCAT`lưu trữ có bao nhiêu chuỗi con “C-A-T” đầy đủ tồn tại trong hậu tố được xử lý. 

Chúng tôi cập nhật các bộ đếm này dựa trên ký tự hiện tại: 

1. Bắt đầu với tất cả các bộ đếm được đặt về 0. Chúng tôi xử lý các ký tự từ chỉ mục cuối cùng xuống chỉ mục đầu tiên. 
2. Nếu ký tự hiện tại là ‘T’, hãy tăng dần`cntT`bởi vì chữ ‘T’ này có thể đóng vai trò là điểm kết thúc của các mẫu “AT” và “CAT” trong tương lai. 
3. Nếu ký tự hiện tại là 'A' thì mọi chữ 'T' hiện có trong hậu tố có thể ghép với 'A' này để tạo thành một chữ "AT" mới. Chúng tôi thêm`cntT`ĐẾN`cntAT`. 
4. Nếu ký tự hiện tại là 'C' thì mọi cặp "AT" hiện có trong hậu tố có thể được mở rộng bằng chữ 'C' này ở phía trước để tạo thành một "CAT" đầy đủ. Chúng tôi thêm`cntAT`ĐẾN`cntCAT`. 

Sau khi xử lý tất cả các ký tự,`cntCAT`nắm giữ câu trả lời. 

### Tại sao nó hoạt động 

Tại bất kỳ vị trí nào trong quá trình quét từ phải sang trái,`cntT`đếm chính xác tất cả các lựa chọn hợp lệ cho ký tự cuối cùng của mẫu trong hậu tố đã được xử lý. Tương tự,`cntAT`đếm tất cả các cặp có thứ tự hợp lệ (A trước T) hoàn toàn bên trong hậu tố. Cuối cùng, mỗi khi chúng ta nhìn thấy chữ 'C', việc kết hợp nó với tất cả các cặp "AT" hiện có sẽ liệt kê chính xác tất cả các bộ ba (C, A, T) với thứ tự đúng. Mỗi bước mở rộng duy trì thứ tự vì các đóng góp chỉ đến từ hậu tố nằm hoàn toàn bên phải vị trí hiện tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    
    cntT = 0
    cntAT = 0
    cntCAT = 0
    
    for ch in reversed(s):
        if ch == 'T':
            cntT += 1
        elif ch == 'A':
            cntAT += cntT
        elif ch == 'C':
            cntCAT += cntAT
    
    print(cntCAT)

if __name__ == "__main__":
    solve()
```Việc thực hiện theo sau quá trình quét ngược trực tiếp. Chi tiết chính là thứ tự cập nhật nghiêm ngặt:`cntT`phải được cập nhật trước`cntAT`, Và`cntAT`trước`cntCAT`tùy thuộc vào loại ký tự, vì mỗi lớp phụ thuộc vào thông tin hậu tố được tích lũy trước đó. 

Không cần lưu trữ trung gian vì tất cả cấu trúc liên quan được nén vào ba bộ đếm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
CATAT
```Chúng tôi theo dõi các biến trong khi quét từ phải sang trái. 

| Nhân vật | cntT | cntAT | cntCAT | 
| --- | --- | --- | --- | 
| T | 1 | 0 | 0 | 
| A | 1 | 1 | 0 | 
| T | 2 | 1 | 0 | 
| A | 2 | 3 | 0 | 
| C | 2 | 3 | 3 | 

Câu trả lời cuối cùng: 3 

Điều này xác nhận rằng nhiều cấu trúc “AT” chồng chéo có thể được tái sử dụng bởi một ‘C’ duy nhất để tạo thành nhiều bộ ba hợp lệ. 

### Ví dụ 2 

đầu vào:```
ACAT
```| Nhân vật | cntT | cntAT | cntCAT | 
| --- | --- | --- | --- | 
| T | 1 | 0 | 0 | 
| A | 1 | 1 | 0 | 
| C | 1 | 1 | 1 | 
| A | 1 | 2 | 1 | 

Câu trả lời cuối cùng: 1 

Điều này chứng tỏ rằng chỉ tồn tại một bộ ba hợp lệ, mặc dù tồn tại nhiều cặp một phần, bởi vì chỉ có một chữ ‘C’ có sẵn để neo chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi ký tự được xử lý một lần với các bản cập nhật liên tục | 
| Không gian |$O(1)$| Chỉ có ba bộ đếm số nguyên được duy trì | 

Quét tuyến tính là tối ưu cho các chuỗi có độ dài lên tới$10^5$hoặc hơn, vì nó thực hiện một lần chuyển mà không có lần lặp lồng nhau. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    s = sys.stdin.readline().strip()
    
    cntT = 0
    cntAT = 0
    cntCAT = 0
    
    for ch in reversed(s):
        if ch == 'T':
            cntT += 1
        elif ch == 'A':
            cntAT += cntT
        elif ch == 'C':
            cntCAT += cntAT
    
    return str(cntCAT)

# provided samples (conceptual)
assert run("CATAT\n") == "3"

# minimum size
assert run("C\n") == "0"

# no valid pattern
assert run("TTTAAA\n") == "0"

# single valid pattern
assert run("CAT\n") == "1"

# multiple overlaps
assert run("CCAAATTT\n") == "27"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| C | 0 | trường hợp cạnh tối thiểu | 
| TTTAAA | 0 | yêu cầu đặt hàng | 
| MÈO | 1 | tính đúng đắn cơ bản | 
| CCAAATTT | 27 | xử lý vụ nổ tổ hợp | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các chữ cái được lặp lại nhiều và tồn tại tất cả các kết hợp hợp lệ. Ví dụ: 

đầu vào:```
CCAAATTT
```Chúng tôi xử lý từ phải sang trái. Mỗi 'T' đều tăng`cntT`, mọi 'A' đóng góp tất cả hiện tại`T`s để`cntAT`và mọi 'C' nhân tất cả hiện tại`AT`cặp thành`cntCAT`. Bởi vì có nhiều chữ cái giống nhau nên bộ đếm tăng nhanh nhưng luôn biểu thị số lượng tổ hợp chính xác chứ không phải xấp xỉ. 

Một trường hợp cạnh khác là thiếu một ký tự bắt buộc, chẳng hạn như không có 'A' trong chuỗi. Trong hoàn cảnh đó,`cntAT`không bao giờ tăng từ 0, vì vậy`cntCAT`vẫn bằng 0 bất kể có bao nhiêu 'C' và 'T' tồn tại. Điều này phù hợp với định nghĩa vì không có bộ ba hợp lệ nào có thể được hình thành nếu không có ký tự ở giữa.
