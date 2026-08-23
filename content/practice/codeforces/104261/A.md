---
title: "CF 104261A - Trạng thái hành tinh"
description: "Chúng ta được cho một số nguyên $n$, đại diện cho bán kính ước tính của Sao Diêm Vương tính bằng dặm. Câu hỏi đặt ra là liệu giá trị này có đủ lớn để Sao Diêm Vương đủ tiêu chuẩn là một hành tinh theo yêu cầu hình học cụ thể hay không."
date: "2026-07-01T23:06:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104261
codeforces_index: "A"
codeforces_contest_name: "UTPC Contest 03-24-23 Div. 2 (Beginner)"
rating: 0
weight: 104261
solve_time_s: 56
verified: true
draft: false
---

[CF 104261A - Trạng thái hành tinh](https://codeforces.com/problemset/problem/104261/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất$n$, đại diện cho bán kính ước tính của Sao Diêm Vương tính bằng dặm. Câu hỏi đặt ra là liệu giá trị này có đủ lớn để Sao Diêm Vương đủ tiêu chuẩn là một hành tinh theo yêu cầu hình học cụ thể hay không. 

Yêu cầu này dựa trên sự so sánh với một đối tượng tham chiếu cố định: một “ngôi sao” có bán kính 1000 dặm. Sao Diêm Vương được mô hình hóa như một vòng tròn và ngôi sao được coi là hình năm cánh. Điều kiện để Sao Diêm Vương đủ điều kiện là một đường tròn có bán kính$n$phải đủ lớn để chứa hoàn toàn ngôi sao. Nói một cách đơn giản hơn, bán kính của Sao Diêm Vương ít nhất phải lớn bằng một giá trị ngưỡng đảm bảo ngôi sao nằm hoàn toàn bên trong nó. 

Vì vậy, nhiệm vụ giảm xuống còn việc quyết định liệu$n$đáp ứng hoặc vượt quá bán kính tối thiểu cần thiết để bao quanh ngôi sao. 

Đầu vào duy nhất là số nguyên này$n$, và đầu ra là một quyết định nhị phân: in "CÓ" nếu Sao Diêm Vương đủ tiêu chuẩn là một hành tinh, nếu không thì in "KHÔNG". 

Ràng buộc$1 \leq n \leq 10^9$ngụ ý rằng giải pháp phải chạy trong thời gian không đổi. Bất kỳ giải pháp nào cố gắng mô phỏng hình học hoặc xây dựng lặp lại hình dạng ngôi sao sẽ không cần thiết và quá chậm, mặc dù bản thân kích thước đầu vào đủ nhỏ nên tính chính xác là thách thức thực sự duy nhất. 

Trường hợp cạnh bị hạn chế nhưng vẫn đáng chú ý. Khi$n = 1000$, Sao Diêm Vương đang ở ngưỡng chính xác và nên được chấp nhận. Khi$n < 1000$, thậm chí bằng 1 đơn vị như$n = 999$, câu trả lời phải là từ chối. Sai lầm phổ biến nhất ở đây là sử dụng sai bất đẳng thức nghiêm ngặt hoặc quên rằng đẳng thức được cho phép. 

## Phương pháp tiếp cận 

Một cách giải thích ngây thơ sẽ cố gắng suy luận về hình dạng thực tế của một ngôi sao năm cánh và tính toán chính xác đường tròn giới hạn của nó. Điều đó sẽ liên quan đến việc lấy tọa độ đỉnh, tính toán khoảng cách từ tâm và sau đó so sánh với bán kính của Sao Diêm Vương. Mặc dù khả thi về mặt toán học, nhưng điều đó là không cần thiết vì bài toán đã cung cấp cái nhìn sâu sắc quan trọng: bán kính ngăn chặn hiệu quả của ngôi sao được cố định ở mức 1000 dặm. 

Phương pháp tiếp cận vũ lực sẽ mô phỏng hoặc tái tạo lại hình dạng ngôi sao, tính toán tất cả các điểm cực trị và sau đó xác định bán kính vòng tròn bao quanh tối thiểu. Điều này sẽ liên quan đến các phép toán hình học có hệ số nặng không đổi và so sánh dấu phẩy động. Mặc dù kích thước đầu vào không liên quan nhưng độ phức tạp khi triển khai cao và gây ra rủi ro về độ ổn định số. 

Sự đơn giản hóa quan trọng là nhận ra rằng kích thước của ngôi sao đã được tóm tắt thành một ngưỡng vô hướng duy nhất: 1000 dặm. Một khi điều đó được chấp nhận, toàn bộ bài toán hình học sẽ chuyển sang dạng so sánh trực tiếp giữa$n$và 1000. 

Vì vậy, thay vì tính toán hình học, chúng ta chỉ cần so sánh bán kính đã cho với ngưỡng. Nếu như$n \geq 1000$, Sao Diêm Vương đủ điều kiện; nếu không thì không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hình học vũ phu | O(1) với hằng số nặng | O(1) | Quá mức cần thiết và không cần thiết | 
| So sánh trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$từ đầu vào. Đây là bán kính ước tính của Sao Diêm Vương. 
2. So sánh$n$với 1000, là bán kính tối thiểu cần thiết để chứa đầy ngôi sao. 
3. Nếu$n \geq 1000$, xuất ra "CÓ" vì vòng tròn của Sao Diêm Vương đủ lớn để thỏa mãn điều kiện ngăn chặn. 
4. Nếu không, xuất ra "NO" vì vòng tròn quá nhỏ để chứa đầy đủ ngôi sao. 

### Tại sao nó hoạt động 

Bài toán quy tất cả độ phức tạp hình học về một điều kiện ngưỡng duy nhất. Sự ngăn chặn bắt buộc của ngôi sao được đặc trưng hoàn toàn bởi bán kính 1000, nghĩa là bất kỳ vòng tròn nào có bán kính dưới 1000 không thể bao quanh nó hoàn toàn, trong khi bất kỳ vòng tròn nào có bán kính ít nhất 1000 đều có thể. Thuật toán đúng vì nó đánh giá trực tiếp điều kiện xác định này mà không cần xấp xỉ hoặc biến đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

if n >= 1000:
    print("YES")
else:
    print("NO")
```Giải pháp đọc một số nguyên duy nhất và thực hiện một so sánh. Chi tiết triển khai chính là đảm bảo so sánh mang tính toàn diện, vì sự bình đẳng được cho phép rõ ràng bởi điều kiện “ít nhất phải lớn bằng 1000”. 

Không cần vòng lặp hoặc phân tích cú pháp bổ sung. Sự đơn giản là có chủ ý: bất kỳ logic bổ sung nào cũng sẽ chỉ tạo cơ hội cho các lỗi sai từng cái một hoặc định dạng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
```| Bước | n | So sánh với 1000 | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 < 1000 | KHÔNG | 

Trường hợp này cho thấy sự thất bại rõ ràng so với ngưỡng. Vì 1 nhỏ hơn 1000 rất nhiều nên ngôi sao không thể nằm trong vòng tròn. 

Đầu ra:```
NO
```### Ví dụ 2 

đầu vào:```
1414
```| Bước | n | So sánh với 1000 | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 1414 | 1414 ≥ 1000 | CÓ | 

Ở đây bán kính vượt quá ngưỡng nên điều kiện ngăn chặn được thỏa mãn. 

Đầu ra:```
YES
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ thực hiện một so sánh số nguyên duy nhất | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Các ràng buộc cho phép các giá trị lên đến$10^9$, nhưng vì quá trình tính toán có thời gian không đổi nên thậm chí nhiều trường hợp thử nghiệm cũng sẽ không đáng kể trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        n = int(sys.stdin.readline().strip())
        print("YES" if n >= 1000 else "NO")
    return out.getvalue().strip()

# provided samples
assert run("1\n") == "NO", "sample 1"
assert run("1414\n") == "YES", "sample 2"

# custom cases
assert run("999\n") == "NO", "just below threshold"
assert run("1000\n") == "YES", "exact threshold"
assert run("1001\n") == "YES", "just above threshold"
assert run("1\n") == "NO", "minimum edge"
assert run("1000000000\n") == "YES", "maximum value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 999 | KHÔNG | hành vi ngay dưới ngưỡng | 
| 1000 | CÓ | ranh giới bình đẳng | 
| 1001 | CÓ | vừa trên ngưỡng | 
| 1 | KHÔNG | trường hợp đầu vào tối thiểu | 
| 1000000000 | CÓ | xử lý giới hạn trên | 

## Vỏ cạnh 

Hành vi cạnh có ý nghĩa duy nhất là xung quanh giá trị ngưỡng 1000. 

Đối với đầu vào$n = 999$, thuật toán đọc giá trị và đánh giá$999 \geq 1000$, sai nên kết quả là "KHÔNG". Điều này phản ánh chính xác rằng vòng tròn là không đủ. 

Đối với đầu vào$n = 1000$, sự so sánh trở thành$1000 \geq 1000$, được đánh giá là đúng, tạo ra "CÓ". Điều này xác nhận rằng sự bình đẳng được coi là sự ngăn chặn hợp lệ. 

Đối với các giá trị lớn như$n = 10^9$, phép so sánh vẫn là một phép kiểm tra số nguyên đơn giản, không có vấn đề về tràn hoặc độ chính xác trong Python, đảm bảo tính chính xác nhất quán trên toàn bộ phạm vi đầu vào.
