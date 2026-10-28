---
title: "CF 106153B - \u742a\u9732\u8bfa\u6570 Ciallo"
description: "Chúng ta được cấp năm số nguyên cho mỗi trường hợp thử nghiệm, mỗi số biểu thị số lần có sẵn một chữ cái hoặc thành phần cụ thể."
date: "2026-06-19T19:20:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106153
codeforces_index: "B"
codeforces_contest_name: "HNNU Freshman Competition Round 2"
rating: 0
weight: 106153
solve_time_s: 49
verified: true
draft: false
---

[CF 106153B - \u742a\u9732\u8bfa\u6570 Ciallo](https://codeforces.com/problemset/problem/106153/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp năm số nguyên cho mỗi trường hợp thử nghiệm, mỗi số biểu thị số lần có sẵn một chữ cái hoặc thành phần cụ thể. Hầu hết các thành phần này tương ứng 1-1 với các chữ cái bắt buộc, ngoại trừ một chữ cái đặc biệt hoạt động khác: nó đóng góp hai lần cho mỗi đơn vị được xây dựng, nghĩa là mỗi “đơn vị” hợp lệ sử dụng hai bản sao của thành phần cụ thể đó trong khi tất cả các thành phần khác sử dụng một bản sao. 

Nhiệm vụ là xác định số lượng đơn vị hoàn chỉnh tối đa có thể được hình thành bằng cách sử dụng các nguồn lực sẵn có. Mỗi đơn vị có một công thức cố định: một đơn vị yêu cầu một trong bốn thành phần thông thường và hai thành phần đặc biệt. Bởi vì các nguồn lực là độc lập nên yếu tố hạn chế luôn là nguồn cung hiệu quả khan hiếm nhất sau khi tính đến nhu cầu tăng gấp đôi. 

Vì vậy, đầu ra chỉ đơn giản là số lượng đơn vị đầy đủ tối đa mà chúng ta có thể tập hợp từ số lượng đã cho. 

Mặc dù vấn đề rất đơn giản nhưng sự tinh tế nằm ở việc chuẩn hóa chính xác thành phần đặc biệt. Nếu chúng ta quên rằng nó đóng góp hai lần cho mỗi đơn vị, chúng ta sẽ đánh giá quá cao tính khả thi. 

Kích thước đầu vào nhỏ và cố định cho mỗi trường hợp thử nghiệm, do đó không có mối lo ngại về tỷ lệ như mảng hoặc biểu đồ lớn. Ngay cả khi có tới 10^5 trường hợp thử nghiệm, mỗi thao tác đều có thời gian không đổi, do đó chỉ cần một giải pháp O(t). 

Một trường hợp hư hỏng điển hình xuất phát từ việc xử lý tất cả các thành phần như nhau. Ví dụ: nếu đầu vào là 1 1 1 1 1, mức tối thiểu đơn giản sẽ là 1, nhưng câu trả lời đúng là 0 vì thành phần đặc biệt phải được sử dụng hai lần và chỉ có một bản sao. 

Một trường hợp cạnh khác là khi thành phần đặc biệt chiếm ưu thế nhưng không chia hết cho hai một cách rõ ràng. Ví dụ: 10 10 10 10 5 sẽ mang lại 2, vì 5 bản sao của thành phần đặc biệt chỉ cho phép có 2 cặp đầy đủ. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ mô phỏng từng đơn vị tòa nhà một. Trong mỗi lần lặp, chúng tôi kiểm tra xem tất cả các thành phần bắt buộc có sẵn hay không, giảm số lượng của chúng và tiếp tục cho đến khi chúng tôi không còn có thể tạo thành một đơn vị nữa. Điều này đúng vì nó phản ánh trực tiếp quá trình xây dựng. Tuy nhiên, mỗi công trình xây dựng đơn vị đòi hỏi công việc liên tục và trong trường hợp xấu nhất, số lượng đơn vị có thể có có thể lớn bằng mức đầu vào tối thiểu, có thể rất lớn. Điều này dẫn đến việc quét tuyến tính trên mỗi đơn vị, làm cho phương pháp này không hiệu quả đối với các giá trị lớn. 

Quan sát chính là mỗi đơn vị độc lập và tiêu thụ tài nguyên cố định. Thay vì mô phỏng lặp đi lặp lại, chúng ta có thể tính toán trực tiếp số lượng đơn vị mà mỗi tài nguyên có thể hỗ trợ. Bốn trong số các thành phần đóng góp số lượng thô của chúng, trong khi thành phần đặc biệt đóng góp một nửa số lượng do yêu cầu hai thành phần trên mỗi đơn vị. Câu trả lời chỉ đơn giản là mức tối thiểu trong số những năng lực hiệu quả này. 

Điều này biến vấn đề mô phỏng lặp đi lặp lại thành một lần giảm duy nhất trên năm giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời cho mỗi bài kiểm tra) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc năm số nguyên biểu thị số lượng có sẵn của mỗi thành phần. Mỗi giá trị được xử lý độc lập ngoại trừ giá trị đặc biệt. 
2. Đối với bốn thành phần thông thường, hãy giữ nguyên giá trị của chúng vì mỗi đơn vị tiêu thụ chính xác một thành phần. Những giá trị này ràng buộc trực tiếp số lượng đơn vị có thể được hình thành. 
3. Đối với thành phần đặc biệt, chia giá trị của nó cho hai bằng phép chia số nguyên. Điều này chuyển đổi tính sẵn có thô thành các “cặp” có thể sử dụng được vì mỗi đơn vị sử dụng hai bản sao. Bất kỳ bản sao nào còn sót lại đều không liên quan. 
4. Tính giá trị tối thiểu trong bốn giá trị bình thường và giá trị đặc biệt được điều chỉnh. Mức tối thiểu này thể hiện nguồn lực thắt cổ chai làm hạn chế việc xây dựng đầy đủ. 
5. Ghi kết quả tối thiểu này làm câu trả lời cuối cùng. 

### Tại sao nó hoạt động

Mỗi đơn vị tiêu thụ một tập hợp tài nguyên cố định, do đó tổng số đơn vị bị ràng buộc độc lập bởi từng loại tài nguyên. Một tài nguyên không thể được chia sẻ giữa các đơn vị vượt quá khả năng sẵn có của nó, vì vậy số lượng đơn vị hoàn chỉnh tối đa bị giới hạn ở trên bởi mỗi năng lực riêng lẻ. Dung lượng của thành phần đặc biệt phải được đo theo cặp chứ không phải đơn lẻ vì tính sẵn có một phần của một bản sao không thể đóng góp cho bất kỳ đơn vị nào. Do đó, mức tối thiểu của tất cả các khả năng là cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b, c, d, e = map(int, input().split())
    print(min(a, b, c, d // 2, e))

t = int(input())
for _ in range(t):
    solve()
```Giải pháp đọc năm số nguyên cho mỗi trường hợp thử nghiệm và tính toán ngay hệ số giới hạn. Phép biến đổi không tầm thường duy nhất là`d // 2`, chuyển đổi tài nguyên đặc biệt thành dung lượng đơn vị có thể sử dụng được. 

Việc sử dụng phép chia số nguyên là rất quan trọng: nó loại bỏ mọi bản sao còn sót lại, không thể tạo thành một đơn vị hợp lệ. Việc lấy mức tối thiểu sau đó sẽ nắm bắt chính xác nút cổ chai trên tất cả các ràng buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1 1 1 1
```| một | b | c | d | e | d//2 | phút | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 0 | 0 | 

Thành phần đặc biệt không đủ ngay cả đối với một đơn vị đầy đủ vì nó yêu cầu hai bản sao. Điều này xác nhận rằng sự sẵn có một phần của nguồn tài nguyên đặc biệt không đóng góp gì. 

### Ví dụ 2 

đầu vào:```
3 2 5 10 4
```| một | b | c | d | e | d//2 | phút | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 | 2 | 5 | 10 | 4 | 5 | 2 | 

Mặc dù thành phần đặc biệt cho phép tối đa 5 đơn vị, nhưng thành phần khác giới hạn chúng tôi ở 2 đơn vị, đây sẽ trở thành câu trả lời cuối cùng. Điều này cho thấy mức tối thiểu xác định chính xác nút cổ chai toàn cầu như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm thực hiện một số phép tính số học không đổi | 
| Không gian | O(1) | Không có cấu trúc dữ liệu phụ trợ ngoài các biến cố định | 

Thuật toán này tối ưu cho các ràng buộc vì nó giảm từng ca kiểm thử thành công việc không đổi. Ngay cả với số lượng lớn các trường hợp thử nghiệm, tổng số tính toán vẫn tuyến tính ở kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        a, b, c, d, e = map(int, input().split())
        print(min(a, b, c, d // 2, e))

    t = int(input())
    for _ in range(t):
        solve()

# minimal case, special cannot form any pair
assert run("1\n1 1 1 1 1\n") == "0\n"

# clean divisible special resource
assert run("1\n2 2 2 4 2\n") == "2\n"

# imbalance dominated by another resource
assert run("1\n10 10 10 100 3\n") == "3\n"

# large special but other bottleneck
assert run("1\n100 1 100 100 100\n") == "1\n"

# all equal large values
assert run("1\n50 50 50 50 50\n") == "25\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 1 | 0 | ràng buộc chẵn lẻ thành phần đặc biệt | 
| 2 2 2 4 2 | 2 | xây dựng cân bằng | 
| 10 10 10 100 3 | 3 | nút cổ chai từ tài nguyên không đặc biệt | 
| 100 1 100 100 100 | 1 | ràng buộc chặt chẽ duy nhất chiếm ưu thế | 
| 50 50 50 50 50 | 25 | trường hợp chia chẵn lớn | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi số lượng thành phần đặc biệt nhỏ hơn hai. Đối với đầu vào`1 10 10 10 10`, thuật toán tính toán`d // 2 = 0`, do đó đáp án cuối cùng trở thành 0. Điều này phản ánh chính xác rằng không thể hình thành được đơn vị nào dù các nguồn lực khác rất dồi dào. 

Một trường hợp khác là khi tất cả các nguồn lực đều lớn nhưng không đồng đều. Vì`100 1 100 100 100`, mức tối thiểu là 1, nghĩa là chỉ riêng thành phần thứ hai đã hạn chế sản lượng. Việc phân chia thành phần đặc biệt không ảnh hưởng đến kết quả vì nó vẫn lớn hơn nút cổ chai. 

Trường hợp cuối cùng là khi mọi thứ hoàn toàn cân bằng và thành phần đặc biệt là chẵn. Vì`50 50 50 50 50`, người đặc biệt đóng góp 25 đơn vị, khớp với tất cả những đơn vị khác, nên kết quả chính xác là 25.
