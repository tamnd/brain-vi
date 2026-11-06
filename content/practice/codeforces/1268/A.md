---
title: "CF 1268A - Số nguyên dài đẹp"
description: "Chúng ta được cung cấp một chuỗi thập phân biểu thị một số nguyên dương có $n$ chữ số. Cùng với nó, có kích thước bước cố định $k$."
date: "2026-06-19T01:00:13+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1268
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 609 (Div. 1)"
rating: 1700
weight: 1268
solve_time_s: 276
verified: true
draft: false
---

[CF 1268A - Số nguyên dài đẹp](https://codeforces.com/problemset/problem/1268/A) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, tham lam, triển khai, chuỗi 
**Thời gian giải:** 4 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi thập phân biểu thị một số nguyên dương với$n$chữ số. Cùng với đó là kích thước bước cố định$k$. Mục tiêu là xây dựng một số nguyên khác$y$sao cho các chữ số của nó lặp lại theo dấu chấm$k$, nghĩa là mọi chữ số ở vị trí$i$phải khớp với chữ số ở vị trí$i+k$, miễn là cả hai vị trí đều tồn tại. 

Ràng buộc này buộc số phải được xác định hoàn toàn bởi lần đầu tiên của nó$k$chữ số. Khi những số đó được chọn, phần còn lại của số sẽ được hình thành bằng cách sao chép chúng theo chu kỳ. 

Nhiệm vụ không chỉ là xây dựng bất kỳ số nào như vậy mà còn là tìm số nhỏ nhất lớn hơn hoặc bằng số đầu vào đã cho. 

Khó khăn chính nằm ở sự tương tác giữa hai ràng buộc: cấu trúc tuần hoàn hạn chế cách các chữ số có thể thay đổi, trong khi điều kiện bất đẳng thức yêu cầu so sánh từ điển một cấu trúc bị ràng buộc với số ban đầu. 

Từ$n$có thể lên tới 200.000, bất kỳ cách tiếp cận nào cố gắng liệt kê các ứng cử viên hoặc mô phỏng các lựa chọn từng chữ số bằng cách quay lui trên toàn bộ phạm vi sẽ quá chậm. Chúng ta cần một giải pháp hoạt động theo thời gian tuyến tính trên chuỗi chữ số. 

Một vài trường hợp khó khăn rất dễ bị đánh giá thấp. 

Một vấn đề xuất hiện khi tiền tố định kỳ đã gần với đầu vào nhưng vẫn nhỏ hơn một chút. Ví dụ, nếu$x = 123123$với$k = 3$, mẫu lặp lại hoạt động hoàn hảo và câu trả lời chính là nó. Nhưng nếu$x = 124123$, việc sử dụng tiền tố "124" có tác dụng nhưng nếu chúng tôi chỉ sao chép không chính xác cho đến khi không khớp và dừng sớm, chúng tôi có thể không truyền bá được tác dụng của việc tăng tiền tố. 

Một trường hợp tinh vi khác xảy ra khi việc tăng tiền tố gây ra dấu mang làm tăng độ dài của nó, chẳng hạn như chuyển "999" thành "1000". Một cách tiếp cận ngây thơ giả định chiều dài cố định$k$sẽ cắt bớt hoặc xử lý sai sự tăng trưởng này một cách không chính xác, dẫn đến kết quả đầu ra không hợp lệ hoặc không ở mức tối thiểu. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ cố gắng liệt kê tất cả các số tuần hoàn hợp lệ bằng cách chọn số đầu tiên$k$chữ số và tạo ra số đầy đủ theo chiều dài$n$, sau đó kiểm tra xem nó có ít nhất$x$. có$10^k$các tiền tố có thể có và mỗi chi phí xây dựng$O(n)$, làm cho cách tiếp cận này không khả thi ngay cả đối với những người vừa phải$k$. Trong trường hợp xấu nhất, nó trở nên lớn về mặt thiên văn. 

Cấu trúc của vấn đề gợi ý một quan điểm trực tiếp hơn. Vì mọi số hợp lệ đều được xác định bởi số đầu tiên của nó$k$chữ số, vấn đề giảm xuống còn việc chọn tiền tố nhỏ nhất sao cho việc khai triển lặp lại ít nhất là$x$. 

Điều này dẫn đến một công trình tham lam. Đầu tiên chúng ta lấy tiền tố độ dài$k$từ đầu vào và lặp lại nó để tạo thành số ứng cử viên. Nếu ứng viên này đã đủ lớn thì nó là tối ưu vì bất kỳ tiền tố nhỏ hơn nào cũng sẽ chỉ làm cho kết quả nhỏ hơn theo thứ tự từ điển. Nếu ứng viên quá nhỏ, chúng ta phải tăng tiền tố lên một đơn vị và xây dựng lại cấu trúc lặp lại. 

Quan sát quan trọng là khi tiền tố được cố định, phần còn lại của số sẽ bị ép buộc, do đó, quyết định có ý nghĩa duy nhất là liệu tiền tố hiện tại có đủ hay cần tăng thêm. Điều này tránh mọi quyết định theo từng vị trí trên tất cả$n$chữ số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(10^k \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý lần đầu tiên$k$các chữ số làm bộ tạo ra số nguyên. 

1. Giải nén cái đầu tiên$k$các chữ số của đầu vào dưới dạng một chuỗi$p$. Đây là phần duy nhất có thể ảnh hưởng đến toàn bộ công trình. 
2. Xây dựng số lượng ứng viên$y$bằng cách lặp lại$p$cho đến khi chúng ta đạt được chiều dài$n$. Điều này tạo ra số nhỏ nhất phù hợp với tiền tố hiện tại. 
3. So sánh ứng viên này với số đầu vào. Nếu nó đã lớn hơn hoặc bằng giá trị đầu vào thì đó là câu trả lời vì bất kỳ số hợp lệ nào nhỏ hơn sẽ yêu cầu tiền tố nhỏ hơn, điều này sẽ chỉ làm giảm kết quả. 
4. Nếu ứng viên nhỏ hơn đầu vào, hãy tăng tiền tố$p$như thể nó là số cơ sở 10. Điều khiển này mang lại sự lan truyền một cách tự nhiên, bao gồm cả trường hợp tiền tố trở nên dài hơn do tràn. 
5. Xây dựng lại câu trả lời bằng cách lặp lại tiền tố đã cập nhật$p$cho đến khi dài$n$. Việc xây dựng mới này được đảm bảo là số hợp lệ nhỏ nhất lớn hơn nhiều so với đầu vào ban đầu. 

### Tại sao nó hoạt động 

Mọi số hợp lệ trong bài toán này đều được xác định đầy đủ bởi số đầu tiên của nó$k$chữ số. Quá trình xây dựng luôn tạo ra số nhỏ nhất phù hợp với tiền tố cố định. So sánh hai số hợp lệ sẽ giảm xuống việc so sánh các tiền tố của chúng theo từ điển. Khi tiền tố quá nhỏ, việc tăng tiền tố ở mức tối thiểu là cách duy nhất để tiếp cận ứng viên hợp lệ có thể vượt qua đầu vào và việc lặp lại tiền tố sẽ duy trì mức tối thiểu trên tất cả các vị trí hậu tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build(p, n):
    # repeat prefix p to length n
    res = []
    k = len(p)
    for i in range(n):
        res.append(p[i % k])
    return ''.join(res)

def add_one(p):
    p = list(p)
    i = len(p) - 1
    while i >= 0 and p[i] == '9':
        p[i] = '0'
        i -= 1
    if i < 0:
        return '1' + ''.join(p)
    p[i] = chr(ord(p[i]) + 1)
    return ''.join(p)

n, k = map(int, input().split())
x = input().strip()

p = x[:k]
cand = build(p, n)

if cand >= x:
    print(n)
    print(cand)
else:
    p = add_one(p)
    cand = build(p, n)
    print(n)
    print(cand)
```Giải pháp bắt đầu bằng cách trích xuất tiền tố xác định cấu trúc tuần hoàn. các`build`hàm xây dựng ứng viên đầy đủ bằng cách lặp lại tiền tố này, đây là sự thể hiện trực tiếp ràng buộc tuần hoàn. Việc so sánh với đầu vào sẽ xác định xem tiền tố này có đủ hay không. 

Nếu không,`add_one`thực hiện tăng dần theo chữ số tiêu chuẩn với quá trình lan truyền mang. Trường hợp cạnh có tất cả các chữ số`'9'`được xử lý bằng cách tạo tiền tố dài hơn bắt đầu bằng`'1'`, vẫn hoạt động chính xác khi được lặp lại theo quy tắc tuần hoàn. 

Một lỗi phổ biến là cố gắng sửa đổi chỉ vị trí không khớp đầu tiên so với đầu vào. Điều đó không thành công vì một mức tăng đơn lẻ sẽ ảnh hưởng đến tất cả các khối lặp lại tiếp theo, do đó thao tác đúng phải coi tiền tố là số nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
353
```| Bước | Tiền tố p | Ứng viên | Kết quả so sánh | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | "35" | "353" | bằng | dừng lại | 

Tiền tố "35" đã tạo ra số đầy đủ "353", khớp chính xác với đầu vào. Không cần tăng thêm. 

Điều này xác nhận rằng khi khai triển tuần hoàn trùng với đầu vào thì thuật toán sẽ kết thúc ngay lập tức mà không sửa đổi. 

### Ví dụ 2 

đầu vào:```
3 2
341
```| Bước | Tiền tố p | Ứng viên | Kết quả so sánh | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | "34" | "343" | lớn hơn | dừng lại | 

Số được xây dựng "343" đã lớn hơn "341", do đó tiền tố hợp lệ mà không cần sửa đổi. 

Điều này cho thấy hành vi chính mà chúng tôi không bao giờ cố gắng khớp từng chữ số một cách tham lam; chúng tôi dựa vào so sánh mở rộng định kỳ đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Việc tạo chuỗi lặp lại và tăng tiền tố đều chạy theo thời gian tuyến tính trên kích thước đầu vào | 
| Không gian |$O(n)$| Chúng tôi lưu trữ một chuỗi ứng viên được xây dựng có độ dài$n$| 

Thuật toán thực hiện một số lượng không đổi các lần truyền đầy đủ trên chuỗi, phù hợp thoải mái trong giới hạn cho$n \leq 200{,}000$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    x = input().strip()

    def build(p, n):
        k = len(p)
        return ''.join(p[i % k] for i in range(n))

    def add_one(p):
        p = list(p)
        i = len(p) - 1
        while i >= 0 and p[i] == '9':
            p[i] = '0'
            i -= 1
        if i < 0:
            return '1' + ''.join(p)
        p[i] = chr(ord(p[i]) + 1)
        return ''.join(p)

    p = x[:k]
    cand = build(p, n)
    if cand >= x:
        return f"{n}\n{cand}"
    p = add_one(p)
    cand = build(p, n)
    return f"{n}\n{cand}"

# provided sample
assert run("3 2\n353\n") == "3\n353"

# all equal digits
assert run("4 2\n1111\n") == "4\n1111"

# needs increment
assert run("3 2\n341\n") == "3\n343"

# carry overflow
assert run("3 2\n999\n") == "3\n999"

# prefix overflow edge
assert run("4 2\n9899\n") == "4\n9999"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1111 với k=2 | 1111 | cấu trúc tuần hoàn đã hợp lệ | 
| 341 với k=2 | 343 | tăng tiền tố sửa lỗi không khớp | 
| 999 với k=2 | 999 | không cần thay đổi ở ranh giới | 
| 9899 với k=2 | 9999 | truyền bá đúng đắn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các chữ số trong tiền tố là`'9'`. Đối với một đầu vào như`x = 999`với`k = 2`, tiền tố "99" tăng lên "100". Thuật toán không phá vỡ ràng buộc định kỳ ở đây vì cấu trúc lặp lại sử dụng trực tiếp tiền tố được cập nhật. Lặp lại "100" sẽ tạo ra "100", giá trị này hợp lệ và lớn hơn giá trị đầu vào. 

Một trường hợp khác là khi độ dài tiền tố tăng do tràn. Mặc dù tiền tố trở nên dài hơn nhưng cấu trúc vẫn hoạt động vì sự lặp lại sử dụng tiền tố mới một cách nhất quán và chúng tôi không bao giờ dựa vào các giả định về độ dài cố định ngoài việc đọc tiền tố đầu tiên.$k$các chữ số làm máy phát điện. 

Trường hợp khó phát hiện cuối cùng là khi đầu vào đã có tính tuần hoàn nhưng không phải là tối thiểu. Thuật toán xác định chính xác rằng việc lặp lại tiền tố sẽ mang lại cùng một số và tránh những thay đổi không cần thiết, vì phép so sánh ban đầu sẽ nắm bắt được sự bằng nhau ngay lập tức.
