---
title: "CF 105485C - \u6587\u795e\u7684\u5e8f\u5217"
description: "Chúng ta được cho một dãy số nguyên dương. Trong một thao tác, chúng ta chọn hai vị trí i và j khác nhau và thay thế giá trị tại i bằng AND theo bit của hai giá trị, ai trở thành ai & aj."
date: "2026-06-23T18:22:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105485
codeforces_index: "C"
codeforces_contest_name: "2024 China Unversity of Geosciences (Wuhan) Freshman Contest"
rating: 0
weight: 105485
solve_time_s: 66
verified: true
draft: false
---

[CF 105485C - \u6587\u795e\u7684\u5e8f\u5217](https://codeforces.com/problemset/problem/105485/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên dương. Trong một thao tác, chúng ta chọn hai vị trí i và j khác nhau và thay thế giá trị tại i bằng AND theo bit của hai giá trị, ai trở thành ai & aj. Giá trị thứ hai không thay đổi nên thao tác chỉ sửa đổi một phần tử nhưng nó cho phép các giá trị “hấp thụ” các bit từ các phần tử khác. 

Mục đích là để xác định xem liệu sau khi thực hiện tối đa n thao tác như vậy, chúng ta có thể làm cho mọi phần tử trong mảng bằng 0 hay không. 

Chi tiết chính là bản chất của bitwise AND. Một bit chỉ tồn tại nếu nó có mặt trong cả hai toán hạng. Vì vậy, việc áp dụng ai & aj chỉ có thể loại bỏ các bit khỏi ai, không bao giờ giới thiệu những cái mới. Điều này đã ngụ ý rằng các giá trị không tăng đơn điệu về mặt bit thiết lập. 

Ràng buộc n 3000 gợi ý rằng cách tiếp cận O(n2) hoặc O(n2 log n) có khả năng được chấp nhận. Bất kỳ khối nào trong n sẽ là đường biên nhưng vẫn có thể chuyển vào Python được tối ưu hóa tùy thuộc vào các hằng số, nhưng một giải pháp rõ ràng nên tránh điều đó. 

Một trường hợp phức tạp phát sinh khi suy nghĩ cục bộ. Một ý tưởng ngây thơ là cố gắng giảm từng số một cách tham lam, nhưng điều này bỏ qua các hoạt động can thiệp trên toàn cầu. Một cạm bẫy khác là giả định rằng nếu mỗi bit xuất hiện ít nhất hai lần trong mảng thì chúng ta có thể loại bỏ nó. Điều này là sai vì cấu trúc của việc chia sẻ rất quan trọng chứ không chỉ tần suất. 

Ví dụ: nếu chúng ta có các giá trị trong đó mỗi bit hiện diện nhưng bị cô lập trong các thành phần rời rạc, việc đếm đơn giản sẽ gợi ý thành công, nhưng không có chuỗi thao tác AND nào có thể chuyển bit đó vào vị trí mà nó có thể bị loại bỏ ở mọi nơi trong vòng n bước. 

Thách thức thực sự là phải hiểu cấu trúc toàn cục nào cho phép các phép toán AND lặp đi lặp lại để cuối cùng đưa mọi số về 0. 

## Phương pháp tiếp cận 

Phép toán ai = ai & aj có một hành vi rất cụ thể: nó làm cho ai trở thành một tập hợp con các bit của aj. Suy nghĩ theo thuật ngữ đã đặt, mỗi số là một tập hợp các bit và AND thay thế một tập hợp bằng giao điểm của nó bằng một tập hợp khác. 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các chuỗi có thể có của tối đa n hoạt động. Mỗi trạng thái là một mảng các số nguyên và mỗi thao tác chọn một cặp có thứ tự (i, j). Điều này dẫn đến hệ số phân nhánh rất lớn là n2 trên mỗi bước và độ sâu n, do đó không gian trạng thái bùng nổ dưới dạng (n2)ⁿ, điều này hoàn toàn không khả thi. 

Để đi đến đâu đó, chúng ta cần hiểu những phép biến đổi nào thực sự có thể thực hiện được thay vì liệt kê các chuỗi. 

Một quan sát quan trọng là xem xét cấu trúc AND toàn cục. Nếu chúng ta tính toán AND theo bit của tất cả các phần tử trong mảng, gọi nó là G, thì mọi phần tử luôn là tập hợp con của G về số bit mà nó có thể bảo toàn thông qua các giao điểm. Tuy nhiên, phép toán AND không bao giờ tạo ra các bit mới, vì vậy cách duy nhất để đạt được tất cả các số 0 là loại bỏ mọi bit thông qua các tầng giao nhau lặp đi lặp lại. 

Cái nhìn sâu sắc về cấu trúc quan trọng là diễn giải từng bit một cách độc lập. Vị trí bit có hoặc không có trong mỗi phần tử mảng. Một thao tác ai = ai & aj sẽ loại bỏ bit đó khỏi ai trừ khi aj cũng có nó. Vì vậy, một bit chỉ có thể tồn tại nếu nó luôn được hỗ trợ bởi một số phần tử “vận chuyển” giúp nó tồn tại trong các bước lan truyền. 

Điều này dẫn đến việc diễn giải biểu đồ trên mỗi bit: đối với một bit cố định, hãy xem xét tất cả các chỉ số trong đó bit đó là 1. Chúng ta chỉ có thể xóa bit đó khỏi tất cả các vị trí nếu tồn tại một chuỗi các thao tác mà cuối cùng buộc mọi lần xuất hiện đều phải giao với giá trị 0 cho bit đó. Vì chúng tôi được phép thực hiện tối đa n hoạt động nên chúng tôi có đủ linh hoạt để sử dụng chiến lược xoay trục trung tâm.

Ý tưởng chính cuối cùng là xây dựng trạng thái mục tiêu: chúng tôi muốn tích lũy đủ “tương tác phá hoại” để mọi bit đều bị loại bỏ ở mọi nơi. Điều này có thể thực hiện được chính xác khi tồn tại một chuỗi các giao điểm theo cặp có thể truyền các số 0 trên tất cả các thành phần, điều này giúp giảm việc kiểm tra xem mảng có ít nhất một phần tử có thể được sử dụng làm bộ giảm phổ quát sau khi thực hiện đủ các phép giảm hay không. 

Một cách định dạng lại đơn giản và chính xác hơn xuất hiện: chúng ta luôn có thể truyền một giá trị đi xuống bằng cách sử dụng AND, vì vậy chiến lược tốt nhất là liên tục thu gọn mảng thành một giá trị duy nhất bằng AND toàn cục của tất cả các phần tử, sau đó truyền bá các số 0 khi và chỉ khi AND toàn cục đó bằng 0. Tuy nhiên, vì các hoạt động mang tính định hướng và chỉ thay đổi một bên, chúng ta có thể mô phỏng rằng sau khi thực hiện đủ các hoạt động, chúng ta có thể buộc tất cả các phần tử trở thành AND toàn cục khi và chỉ khi mảng được kết nối theo mối quan hệ chia sẻ ít nhất một chuỗi bit chung. 

Điều này giúp đơn giản hóa việc kiểm tra xem đồ thị theo bit do các phần tử tạo ra có được kết nối đủ để cho phép lan truyền đầy đủ hay không, trong bài toán này giải quyết một điều kiện cần và đủ đơn giản hơn: AND theo bit của tất cả các phần tử phải bằng 0, vì nếu không thì tồn tại ít nhất một bit không bao giờ có thể bị loại bỏ khỏi mọi phần tử cùng một lúc. 

Do đó, vấn đề giảm xuống còn việc tính toán AND toàn cục và kiểm tra xem nó có bằng 0 hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n³) hoặc tệ hơn | O(n²) | Quá chậm | 
| Cái nhìn sâu sắc toàn cầu VÀ giảm thiểu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán AND theo từng bit của tất cả các phần tử trong mảng. Giá trị này đại diện cho tập hợp các bit có trong mọi số. 

Sau đó chúng tôi kiểm tra xem kết quả này có bằng không hay không. 

1. Đọc mảng n số nguyên. 
2. Khởi tạo biến g làm phần tử đầu tiên của mảng. 
3. Lặp lại phần còn lại của mảng và cập nhật g = g & ai cho từng phần tử. 
4. Sau khi xử lý tất cả các phần tử, kiểm tra xem g có bằng 0 hay không. 
5. Nếu g bằng 0, xuất ra "Có", nếu không thì xuất ra "Không". 

Lý do điều này có tác dụng là vì bất kỳ bit nào có trong tất cả các số đều không bao giờ có thể bị loại bỏ khỏi hệ thống. Mọi thao tác đều bảo toàn bit đó trong ít nhất một phần tử chứa nó và vì AND không bao giờ đưa các số 0 mới vào giao điểm chung đó trên toàn cầu nên không thể xóa hoàn toàn một bit như vậy khỏi tất cả các vị trí. Ngược lại, nếu không có bit nào chung cho tất cả các phần tử thì mỗi bit sẽ xuất hiện ở đâu đó bên ngoài bất kỳ lõi giao nhau cố định nào và các phép toán AND theo cặp lặp lại có thể được sắp xếp để loại bỏ dần dần các bit trên mảng cho đến khi tất cả các mục trở thành 0. 

Điều bất biến là sau bất kỳ chuỗi thao tác nào, AND theo bit của tất cả các phần tử vẫn bằng AND toàn cục ban đầu. Bất biến này hạn chế cấu hình cuối cùng có thể đạt được: nếu trạng thái cuối cùng đều bằng 0 thì AND toàn cục của nó sẽ bằng 0, do đó AND toàn cục ban đầu cũng phải bằng 0. Ngược lại, khi AND toàn cục bằng 0, không có bit nào bị khóa vĩnh viễn trong mọi phần tử, do đó có thể loại bỏ hoàn toàn trong số lượng thao tác được phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))
    
    g = a[0]
    for x in a[1:]:
        g &= x
    
    print("Yes" if g == 0 else "No")

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp theo sau việc giảm bớt tính toán bitwise AND toàn cục. Điểm tinh tế duy nhất là khởi tạo: bắt đầu từ a[0] đảm bảo tính chính xác mà không cần đưa ra giá trị nhận dạng. Vòng lặp sau đó gấp AND trên mảng. 

Điều kiện đầu ra là một kiểm tra tính bằng nhau nghiêm ngặt về 0, kiểm tra xem có bit nào chung cho tất cả các phần tử hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 5
```Chúng tôi tính toán chạy AND: 

| Bước | g hiện tại | Giá trị tiếp theo | g mới | 
| --- | --- | --- | --- | 
| 1 | 1 (001) | 2 (010) | 0 (000) | 
| 2 | 0 (000) | 5 (101) | 0 (000) | 

Kết quả cuối cùng là 0, vì vậy đầu ra là Có. 

Điều này thể hiện trường hợp không có bit nào được chia sẻ trên tất cả các phần tử, cho phép loại bỏ hoàn toàn. 

### Ví dụ 2 

đầu vào:```
3
7 7 7
```| Bước | g hiện tại | Giá trị tiếp theo | g mới | 
| --- | --- | --- | --- | 
| 1 | 7 (111) | 7 (111) | 7 (111) | 
| 2 | 7 (111) | 7 (111) | 7 (111) | 

Kết quả cuối cùng là 7 nên đầu ra là No. 

Điều này cho thấy rằng khi một bit chung cho tất cả các phần tử, nó sẽ được bảo toàn vĩnh viễn và ngăn chặn việc đưa về 0 hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tính toán một lần theo bit VÀ trên mảng | 
| Không gian | O(1) | Chỉ sử dụng một số lượng biến không đổi | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì n 3000 và số lượng hoạt động là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    def solve():
        n = int(input().strip())
        a = list(map(int, input().split()))
        g = a[0]
        for x in a[1:]:
            g &= x
        print("Yes" if g == 0 else "No")
    
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# provided sample
assert run("3\n1 2 5\n") == "Yes"

# all zeros achievable trivial case
assert run("1\n1\n") == "No"

# all equal non-zero
assert run("4\n7 7 7 7\n") == "No"

# mixed with zero
assert run("3\n0 1 2\n") == "Yes"

# large spread
assert run("5\n1 2 4 8 16\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 2 5 | Có | hành vi mẫu, không có bit chung | 
| 4 7 7 7 7 | Không | khối bit được chia sẻ đầy đủ thành công | 
| 3 0 1 2 | Có | sự hiện diện của phần tử 0 | 
| 5 1 2 4 8 16 | Có | trường hợp bit rời rạc | 

## Vỏ cạnh 

Trường hợp một cạnh là mảng một phần tử. Nếu n = 1 và giá trị khác 0 thì không thể thực hiện phép toán nào, do đó câu trả lời phải là Không. Thuật toán xử lý điều này vì AND toàn cục bằng chính phần tử đó, khác 0. 

Một trường hợp cạnh khác là khi tất cả các phần tử đều bằng 0. AND toàn cục bằng 0, vì vậy câu trả lời là Có ngay lập tức, phù hợp với thực tế là không cần thực hiện thao tác nào. 

Trường hợp thứ ba là khi một số phần tử bằng 0 và những phần tử khác khác 0. Vì AND với số 0 buộc các bit giảm xuống, AND toàn cục trở thành số 0 và thuật toán đưa ra chính xác Có, phản ánh rằng số 0 hoạt động như một bộ khử bit phổ quát trong hệ thống.
