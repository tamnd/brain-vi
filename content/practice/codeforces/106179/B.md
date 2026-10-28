---
title: "CF 106179B - Giả Palindrome"
description: "Bài toán hỏi liệu một mảng có thể được sắp xếp lại sao cho mọi cặp phần tử được đặt đối xứng từ hai đầu đều đủ gần hay không."
date: "2026-06-25T10:54:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106179
codeforces_index: "B"
codeforces_contest_name: "ICPC India Online Prelims (2025 - 2026)"
rating: 0
weight: 106179
solve_time_s: 35
verified: true
draft: false
---

[CF 106179B - Giả Palindrome](https://codeforces.com/problemset/problem/106179/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán hỏi liệu một mảng có thể được sắp xếp lại sao cho mọi cặp phần tử được đặt đối xứng từ hai đầu đều đủ gần hay không. Sau khi chọn một số hoán vị của mảng, giá trị đầu tiên và cuối cùng phải khác nhau nhiều nhất`d`, giá trị thứ hai và thứ hai cuối cùng phải khác nhau nhiều nhất`d`, vân vân. Nhiệm vụ chỉ là quyết định xem thứ tự đó có tồn tại hay không. Vấn đề ban đầu là từ Codeforces Gym 106179. 

Đầu vào chứa một số trường hợp thử nghiệm. Mỗi test case đưa ra độ dài của mảng, chênh lệch tối đa cho phép`d`và các giá trị trong mảng. Đầu ra là`YES`nếu một số sắp xếp lại thỏa mãn tất cả các điều kiện cặp đối xứng, nếu không thì đó là`NO`. 

Tổng số phần tử mảng trong tất cả các trường hợp thử nghiệm tối đa là 2000 và một mảng riêng lẻ có thể có độ dài 2000. Điều này có nghĩa là`O(n^2)`giải pháp là có thể về mặt kỹ thuật, nhưng sẽ hữu ích khi tìm kiếm một cấu trúc đơn giản hơn vì hoạt động đang được kiểm tra dựa trên thứ tự. Bản thân các giá trị có thể lớn bằng`10^9`, vì vậy các giải pháp nên tránh các giả định về dải tần hoặc dải tần nhỏ. 

Một lỗi phổ biến là chỉ kiểm tra xem giá trị tối thiểu và tối đa có nằm trong khoảng cách không`d`. Điều kiện đó là cần nhưng chưa đủ. Ví dụ:```
1
4 5
1 4 10 12
```Mức tối thiểu và tối đa khác nhau bởi`11`, vì vậy điều này đã thất bại, nhưng có thể xảy ra một lỗi khác khi các cực trị đều ổn nhưng các cặp bên trong lại thất bại. Coi như:```
1
6 5
1 5 6 10 11 12
```Giá trị nhỏ nhất và lớn nhất khác nhau bởi`11`, nên điều này rõ ràng là không thể được. Trường hợp cạnh hữu ích hơn là một phần tử duy nhất:```
1
1 0
100
```Câu trả lời là`YES`. Không có cặp nào vi phạm điều kiện và bất kỳ triển khai nào kiểm tra một cách mù quáng chỉ mục được phản chiếu mà không xử lý vị trí trung tâm đều có thể từ chối nó một cách không chính xác. 

Một trường hợp cạnh khác là khi`d`là số không. Ví dụ:```
1
4 0
7 7 7 8
```Câu trả lời là`NO`. Mỗi cặp được nhân đôi phải chứa các giá trị chính xác bằng nhau và giá trị đơn`8`không thể sánh được. Giải pháp chỉ kiểm tra sự khác biệt lân cận sau khi sắp xếp có thể bỏ sót điều này vì mảng gần như đồng nhất. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử sắp xếp lại có thể và kiểm tra xem mọi cặp đối xứng có hợp lệ hay không. Điều này đúng vì nó kiểm tra chính xác điều kiện mà bài toán yêu cầu. Tuy nhiên, số lượng đơn đặt hàng có thể là`n!`, điều này trở nên không thể ngay cả đối với các mảng nhỏ. 

Một lực lượng vũ phu hợp lý hơn sẽ sắp xếp mảng và cố gắng xây dựng các cặp một cách tham lam hoặc sử dụng tính năng quay lui trên các cặp. Vấn đề là số lượng các cặp bài tập có thể thực hiện được vẫn tăng quá nhanh. Với`n = 2000`, thậm chí một`O(n^2)`Cách tiếp cận này đã có khoảng bốn triệu hoạt động, trong khi bất cứ điều gì liên quan đến việc khám phá nhiều cặp đôi đều vượt quá giới hạn. 

Quan sát quan trọng là chỉ có khoảng cách lớn nhất bên trong một cặp mới quan trọng. Nếu hai giá trị cách xa nhau thì không nên đặt chúng cạnh nhau. Việc sắp xếp sẽ hiển thị ngay lập tức cặp hạn chế nhất: phần tử lớn nhất phải được ghép với một số phần tử ở đầu dưới của mảng. Phần tử nhỏ nhất có cùng một vấn đề. 

Sự sắp xếp tối ưu liên tục ghép giá trị còn lại nhỏ nhất với giá trị còn lại lớn nhất. Đây không chỉ là một công trình thuận tiện. Nó mang lại sự khác biệt cặp tối đa nhỏ nhất có thể trong số tất cả các cách để chia mảng thành các cặp được nhân đôi. Nếu sự sắp xếp này thất bại thì không có sự sắp xếp lại nào khác có thể làm cho các giá trị cực trị phù hợp vì việc di chuyển một trong hai cực vào trong sẽ buộc một giá trị khác thế chỗ và tạo ra một khoảng cách bằng hoặc lớn hơn. 

Sau khi sắp xếp mảng như`b`, việc kiểm tra duy nhất cần thiết là liệu:`b[n - 1 - i] - b[i] <= d`cho mọi chỉ mục được nhân đôi`i`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Sắp xếp và kiểm tra các cặp được nhân đôi | O(n log n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm. Việc sắp xếp sẽ đưa ra những cặp cực đoan nhất có thể, đó là những cặp quyết định liệu sự sắp xếp có thể tồn tại hay không. 
2. So sánh phần tử nhỏ nhất với phần tử lớn nhất. Nếu sự khác biệt của chúng lớn hơn`d`, câu trả lời là ngay lập tức`NO`, bởi vì cả hai thái cực này đều không thể được đặt vào các vị trí đối xứng hợp lệ. 
3. Tiếp tục hướng vào trong bằng cách so sánh cái nhỏ thứ hai với cái lớn thứ hai, rồi cái nhỏ thứ ba với cái lớn thứ ba, cho đến khi đến giữa. Mỗi so sánh đại diện cho một cặp phản chiếu có thể có trong sự sắp xếp tốt nhất. 
4. Nếu tất cả các cặp đối xứng đều thỏa mãn giới hạn khoảng cách, đầu ra`YES`. Bản thân sự sắp xếp nhân đôi được sắp xếp là một công trình hợp lệ. 

Tại sao nó hoạt động: 

Sau khi sắp xếp, hãy xem xét phần tử lớn nhất. Nó cần một đối tác ở khoảng cách xa`d`. Ghép nối nó với bất kỳ thứ gì ngoại trừ một trong những giá trị lớn nhất còn lại có thể chỉ có thể làm cho đối tác trở nên nhỏ hơn và sự khác biệt lớn hơn. Lập luận tương tự áp dụng cho phần tử nhỏ nhất. Việc loại bỏ hai giá trị đó sẽ để lại cùng một vấn đề trên một mảng nhỏ hơn, do đó việc ghép nối các cực trị liên tục sẽ mang lại sự sắp xếp tốt nhất có thể. Nếu ngay cả cách sắp xếp này cũng chứa một cặp không hợp lệ thì mọi cách sắp xếp khác cũng phải chứa một cặp không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, d = map(int, input().split())
        a = list(map(int, input().split()))

        a.sort()

        ok = True
        for i in range(n // 2):
            if a[n - 1 - i] - a[i] > d:
                ok = False
                break

        ans.append("YES" if ok else "NO")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Đầu vào được đọc bằng cách sử dụng`sys.stdin.readline`vì có thể có nhiều trường hợp thử nghiệm. Giải pháp sắp xếp từng mảng trước, đây là thao tác duy nhất thay đổi thứ tự. 

Vòng lặp chỉ kiểm tra nửa đầu của mảng đã được sắp xếp. Mỗi chỉ số`i`được ghép nối với`n - 1 - i`, đó là vị trí được phản chiếu của nó trong phiên bản được sắp xếp. Phần tử ở giữa của mảng có kích thước lẻ bị bỏ qua vì nó không có phần tử đối tác. 

Việc so sánh sử dụng phép trừ giữa các số nguyên Python, vì vậy các giá trị lớn lên đến`10^9`được xử lý một cách an toàn. Việc thoát sớm sau khi tìm thấy một cặp không hợp lệ sẽ tránh được những kiểm tra không cần thiết, mặc dù độ phức tạp tiệm cận vẫn giữ nguyên. 

## Ví dụ đã hoạt động 

Mẫu 1:```
3
1 1
1
2 0
1 2
2 1000
1 2
```| Kiểm tra | Mảng được sắp xếp | Cặp đã kiểm tra | Kết quả | 
| --- | --- | --- | --- | 
| 1 | [1] | Không có cặp | CÓ | 
| 2 | [1, 2] | 2 - 1 = 1 > 0 | KHÔNG | 
| 3 | [1, 2] | 2 - 1 = 1 <= 1000 | CÓ | 

Trường hợp đầu tiên xác nhận rằng một giá trị duy nhất luôn thành công. Trường hợp thứ hai cho thấy tại sao cần có các giá trị bằng nhau khi`d`là số không. Trường hợp thứ ba cho thấy giới hạn đủ lớn cho phép hai giá trị được phản ánh. 

Mẫu 2: 

đầu vào:```
1
6 5
1 5 6 10 11 12
```| Bước | Chỉ số trái | Chỉ mục bên phải | Sự khác biệt | Tiểu bang | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 5 | 12 - 1 = 11 | Thất bại | 

Sự so sánh đầu tiên đã thất bại. Vì giá trị nhỏ nhất và lớn nhất không thể là cặp phản chiếu hợp lệ nên không có sự sắp xếp lại nào có thể giải được bài kiểm tra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế trong quá trình quét tuyến tính của các cặp được nhân đôi. | 
| Không gian | O(1) thêm | Thuật toán chỉ lưu trữ mảng đầu vào và sử dụng một vài biến. | 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm chỉ là 2000, do đó việc sắp xếp dễ dàng trong giới hạn cho phép. Cách tiếp cận này cũng hoạt động thoải mái đối với các mảng lớn hơn nhiều. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

assert run("""3
1 1
1
2 0
1 2
2 1000
1 2
""") == """YES
NO
YES
""", "samples"

assert run("""1
1 0
42
""") == """YES
""", "single element"

assert run("""1
4 0
7 7 7 8
""") == """NO
""", "all equal except one value"

assert run("""1
5 3
1 3 5 7 9
""") == """NO
""", "middle values cannot repair extremes"

assert run("""1
6 10
1 2 5 8 9 10
""") == """YES
""", "larger valid range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng phần tử đơn | CÓ | Vỏ trung tâm không có cặp gương. | 
| Một giá trị khác với`d = 0`| KHÔNG | Mỗi cặp phải giống hệt nhau. | 
| Phạm vi rộng với kích thước nhỏ`d`| KHÔNG | Giá trị cực đoan quyết định không thể. | 
| Khoảng cách đủ cho mọi cặp cực đoan | CÓ | Chấp nhận chính xác việc ghép nối được sắp xếp hợp lệ. | 

## Vỏ cạnh 

Đối với trường hợp phần tử đơn:```
1
1 0
42
```Mảng được sắp xếp là`[42]`. Vòng lặp thực hiện lần lặp 0 vì`n // 2`là số không. Đầu ra của thuật toán`YES`, phù hợp với thực tế là không có cặp nào có thể vi phạm điều kiện. 

Đối với trường hợp khoảng cách bằng không:```
1
4 0
7 7 7 8
```Sau khi sắp xếp, mảng là`[7, 7, 7, 8]`. Kiểm tra so sánh được nhân đôi đầu tiên`8 - 7 = 1`, lớn hơn 0 nên thuật toán dừng và in`NO`. Điều chưa từng có`8`không thể bị ẩn đi bằng cách sắp xếp lại vì nó phải thuộc về một cặp đối xứng nào đó. 

Đối với trường hợp chỉ kiểm tra mức tối thiểu và tối đa toàn cầu là không đủ, thuật toán sẽ kiểm tra mọi lớp được phản chiếu. Ví dụ:```
1
5 3
1 3 5 7 9
```Mảng được sắp xếp vẫn giữ nguyên. Cặp bên ngoài cho`9 - 1 = 8`, đã quá lớn nên câu trả lời là`NO`. Ngay cả khi các cực trị là hợp lệ, các so sánh bên trong vẫn sẽ được kiểm tra, ngăn chặn việc chấp nhận sai từ một điều kiện từng phần.
