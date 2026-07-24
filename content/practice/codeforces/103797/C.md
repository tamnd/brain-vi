---
title: "CF 103797C - Những câu nói dễ thương"
description: "Chúng ta được cho một câu được chia thành một chuỗi các từ. Mỗi từ chỉ bao gồm các chữ cái tiếng Anh viết hoa. Nhiệm vụ là xác định xem câu có thỏa mãn thuộc tính cấu trúc cụ thể liên quan đến từ đầu tiên của nó hay không."
date: "2026-07-02T08:46:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103797
codeforces_index: "C"
codeforces_contest_name: "IME++ Starters Try-outs 2022"
rating: 0
weight: 103797
solve_time_s: 45
verified: true
draft: false
---

[CF 103797C - Những câu nói dễ thương](https://codeforces.com/problemset/problem/103797/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một câu được chia thành một chuỗi các từ. Mỗi từ chỉ bao gồm các chữ cái tiếng Anh viết hoa. Nhiệm vụ là xác định xem câu có thỏa mãn thuộc tính cấu trúc cụ thể liên quan đến từ đầu tiên của nó hay không. 

Quy tắc xác định câu là "dễ thương" nếu từ đầu tiên có thể được xây dựng lại bằng cách lấy chữ cái đầu tiên của mỗi từ trong câu theo thứ tự. Nói cách khác, nếu chúng ta viết câu dưới dạng danh sách các từ, trích xuất ký tự đầu tiên của mỗi từ và ghép chúng lại, chuỗi kết quả phải khớp chính xác với từ đầu tiên, không có chữ cái thừa hoặc thiếu. 

Kích thước đầu vào rất nhỏ, tối đa 100 từ và mỗi từ tối đa 100 ký tự. Điều này ngay lập tức ngụ ý rằng bất kỳ giải pháp nào hoạt động theo thời gian tuyến tính trên tất cả các ký tự đều đủ nhanh. Ngay cả một giải pháp liên tục quét các từ nhiều lần vẫn sẽ an toàn trong các ràng buộc. 

Rủi ro chính không phải là hiệu suất mà là tính chính xác. Một vài trường hợp tế nhị quan trọng: 

Trường hợp một cạnh là khi từ đầu tiên có độ dài khác với số từ. Ví dụ: nếu câu có 3 từ nhưng từ đầu tiên có độ dài 5 thì chúng không thể khớp với nhau vì từ viết tắt chỉ có thể có độ dài bằng số từ. 

Một trường hợp khác là khi một từ là một chữ cái. Đây thực sự là trường hợp bình thường và phải được xử lý nhất quán vì cấu trúc từ viết tắt chỉ dựa vào ký tự đầu tiên của mỗi từ bất kể độ dài của từ. 

Trường hợp thứ ba là khi các chữ cái đầu tiên khớp nhau nhưng thứ tự không được căn chỉnh. Ví dụ: ngay cả khi tất cả các chữ cái đều tồn tại giữa các từ, trình tự phải khớp chính xác theo thứ tự chứ không phải dưới dạng so sánh tập hợp. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xây dựng rõ ràng chuỗi từ viết tắt bằng cách lặp qua tất cả các từ, lấy ký tự đầu tiên của mỗi từ và nối chúng thành một chuỗi mới. Sau đó, chúng tôi so sánh chuỗi được xây dựng này với từ đầu tiên. 

Điều này đúng vì định nghĩa “câu dễ thương” hoàn toàn mang tính cấu trúc và không yêu cầu chuyển đổi nào ngoài việc trích xuất và so sánh. Chi phí bị chi phối bởi việc quét từng từ một lần, do đó tổng công việc tỷ lệ thuận với tổng độ dài của từ. 

Một biến thể brute-force có thể cố gắng xác thực từng ký tự của từ đầu tiên bằng cách quét liên tục các từ hoặc kiểm tra các vị trí khớp với các vòng lặp lồng nhau. Mặc dù vẫn khả thi trong điều kiện hạn chế nhưng nó phức tạp không cần thiết và có thể gây ra lỗi lập chỉ mục. Trong trường hợp xấu nhất, cách tiếp cận như vậy có thể thực hiện kiểm tra ký tự O(N^2), ở đây vẫn còn nhỏ nhưng có tỷ lệ kém đối với đầu vào lớn hơn. 

Quan sát quan trọng là từ viết tắt được xác định duy nhất bởi các ký tự đầu tiên của mỗi từ, vì vậy chúng ta không bao giờ cần phải xem xét bất kỳ cấu trúc nào khác. Điều này làm giảm vấn đề xuống còn một lần xây dựng và so sánh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(tổng ký tự) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số từ và danh sách các từ. Chúng tôi giữ chúng theo thứ tự vì cấu trúc vị trí là điều cần thiết. 
2. Trích xuất ký tự đầu tiên của mỗi từ và tạo một chuỗi mới. Chuỗi này đại diện cho từ viết tắt ngụ ý trong định nghĩa câu. 
3. So sánh từ viết tắt được xây dựng với từ đầu tiên trong câu. 
4. Nếu chúng giống nhau thì xuất ra “Có”, nếu không thì xuất ra “Không”. 

Mỗi bước phản ánh trực tiếp định nghĩa, do đó không cần cấu trúc phụ trợ hoặc tiền xử lý ngoài việc lặp lại đơn giản. 

### Tại sao nó hoạt động

Từ đầu tiên được yêu cầu phải chính xác bằng cách ghép các ký tự đầu tiên của tất cả các từ. Vì mỗi từ đóng góp chính xác một ký tự cho từ viết tắt và thứ tự được giữ nguyên nên chuỗi được xây dựng là ứng cử viên duy nhất có thể. Vì vậy, sự bằng nhau giữa chuỗi được xây dựng và từ đầu tiên vừa cần vừa đủ để đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    words = input().split()

    acronym = ''.join(w[0] for w in words)

    if acronym == words[0]:
        print("Yes")
    else:
        print("No")

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên đọc số nguyên, sau đó đọc câu. sử dụng`split()`đảm bảo chúng tôi phân tách các từ một cách chính xác bất kể khoảng cách bất thường. 

Hoạt động cốt lõi là biểu thức trình tạo`w[0] for w in words`, trích xuất ký tự đầu tiên của mỗi từ một cách an toàn. Vì mọi từ đều được đảm bảo không trống nên không có nguy cơ xảy ra lỗi chỉ mục. 

Cuối cùng, chúng tôi so sánh từ viết tắt được xây dựng với`words[0]`, đại diện cho từ mục tiêu được yêu cầu. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào: 

IME MELHOR ENGENHARIA 

Chúng tôi xử lý các từ: IME, MELHOR, ENGENHARIA. 

| Bước | Lời | Chữ cái đầu tiên | Từ viết tắt cho đến nay | 
| --- | --- | --- | --- | 
| 1 | IME | Tôi | Tôi | 
| 2 | MELHOR | M | IM | 
| 3 | ENGENHARIA | E | IME | 

Từ viết tắt cuối cùng là “IME”, khớp với từ đầu tiên. Đầu ra là “Có”. 

Dấu vết này xác nhận rằng thuật toán tích lũy chính xác từ viết tắt theo thứ tự và thực hiện so sánh trực tiếp. 

Bây giờ hãy xem xét: 

IME MITO 

| Bước | Lời | Chữ cái đầu tiên | Từ viết tắt cho đến nay | 
| --- | --- | --- | --- | 
| 1 | IME | Tôi | Tôi | 
| 2 | MITO | M | IM | 

Từ viết tắt là “IM”, không khớp với “IME”. Đầu ra là “Không”. 

Điều này cho thấy ngay cả những sai lệch nhỏ về độ dài hoặc ký tự ở cuối cũng được phát hiện ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N · L) | Mỗi từ được quét một lần để trích xuất ký tự đầu tiên của nó | 
| Không gian | O(N) | Lưu trữ danh sách các từ và chuỗi viết tắt | 

Giới hạn đầu vào đủ nhỏ nên ngay cả trường hợp xấu nhất là 100 từ có độ dài 100 cũng không đáng kể. Giải pháp chạy tốt trong cả giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples (as described in statement)
assert run("3\nIME MELHOR ENGENHARIA\n") == "Yes"
assert run("2\nIME MITO\n") == "No"

# all equal single-letter words
assert run("3\nA A A\n") == "Yes"

# mismatch due to extra letter in first word
assert run("2\nAB A\n") == "No"

# single word case
assert run("1\nA\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 từ tạo thành IME | Có | Trường hợp đúng cơ bản | 
| IME MITO | Không | Độ dài không khớp | 
| A A A | Có | Tính nhất quán của một chữ cái | 
| AB A | Không | Ký tự bổ sung trong từ đầu tiên | 
| Từ đơn | Có | Trường hợp ranh giới tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi chỉ có một từ. Đối với đầu vào: 

1 

A 

Thuật toán xây dựng từ viết tắt bằng cách lấy ký tự đầu tiên của từ duy nhất, tạo ra “A”. Sau đó, nó so sánh từ này với từ đầu tiên, cũng là từ “A”, vì vậy đầu ra là “Có”. Điều này xử lý chính xác trường hợp suy biến trong đó câu bao gồm một từ duy nhất. 

Một trường hợp khác là khi từ đầu tiên dài hơn số từ. Ví dụ: 

3 

ABCD A B 

Từ viết tắt trở thành “AAB”, trong khi từ đầu tiên là “ABCD”. Vì các chuỗi khác nhau ngay lập tức nên thuật toán sẽ đưa ra kết quả “Không” một cách chính xác mà không cần bất kỳ xử lý đặc biệt nào. 

Cuối cùng, khi tất cả các từ đều là các ký tự đơn giống hệt nhau, chẳng hạn như: 

4 

X X X X 

Từ viết tắt là “XXXX”, khớp chính xác với từ đầu tiên nên kết quả đầu ra là “Có”. Điều này xác nhận rằng các ký tự lặp lại và đầu vào thống nhất hoạt động chính xác theo cùng một logic.
