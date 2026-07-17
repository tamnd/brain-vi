---
title: "CF 103433D - Mảng tương tự"
description: "Chúng ta được cung cấp một mảng và được yêu cầu quyết định khái niệm về sự giống nhau giữa các mảng theo một quy tắc chuyển đổi cụ thể."
date: "2026-07-03T07:56:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103433
codeforces_index: "D"
codeforces_contest_name: "2018-2019 Russia Team Open, High School Programming Contest (VKOSHP 18)"
rating: 0
weight: 103433
solve_time_s: 40
verified: true
draft: false
---

[CF 103433D - Mảng tương tự](https://codeforces.com/problemset/problem/103433/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và được yêu cầu quyết định khái niệm về sự giống nhau giữa các mảng theo một quy tắc chuyển đổi cụ thể. Vấn đề cơ bản là so sánh cách hai chuỗi hành xử khi bạn được phép chuẩn hóa hoặc “nén” cấu trúc của chúng một cách nhất quán và sau đó kiểm tra xem các biểu diễn kết quả có khớp hay không. 

Mỗi trường hợp thử nghiệm cung cấp hai mảng có độ dài bằng nhau. Nhiệm vụ là xác định xem các mảng này có thể được coi là tương đương trong một quy trình bỏ qua các giá trị chính xác và thay vào đó chỉ bảo toàn cấu trúc tương đối được tạo ra bởi các mẫu sắp xếp và lặp lại hay không. Nói cách khác, hai mảng tương tự nhau nếu chúng tạo ra cùng một kiểu về cách các giá trị mới xuất hiện khi bạn quét từ trái sang phải. 

Để làm rõ điều này, hãy tưởng tượng đọc một mảng từ trái sang phải và thay thế từng giá trị riêng biệt bằng chỉ mục xuất hiện đầu tiên trong mảng đó. Điều này tạo ra một “chữ ký hình dạng” chuẩn cho mảng. Vấn đề giảm xuống còn việc kiểm tra xem cả hai mảng có tạo ra chữ ký giống hệt nhau hay không. 

Các ràng buộc ngụ ý rằng chúng ta phải xử lý các mảng có tiềm năng lớn, do đó, mọi so sánh bậc hai của các cấu trúc con hoặc căn chỉnh theo cặp giữa tất cả các phần tử sẽ quá chậm. Quét tuyến tính trên mỗi mảng là hướng khả thi duy nhất. 

Các trường hợp biên quan trọng xung quanh các giá trị lặp lại và lần xuất hiện đầu tiên. Ví dụ, hãy xem xét: 

đầu vào: 

A = [5, 7, 5] 

B = [1, 2, 1] 

Cả hai mảng đều tạo ra cùng một chữ ký mẫu [0, 1, 0]. Sự so sánh đơn giản về từng phần tử sẽ thất bại vì các giá trị khác nhau, mặc dù về mặt cấu trúc chúng giống hệt nhau. 

Một trường hợp tinh vi khác phát sinh khi cấu trúc lặp lại được giữ nguyên nhưng thứ tự xuất hiện đầu tiên lại khác: 

A = [1, 2, 3, 2] 

B = [4, 5, 6, 5] 

Cả hai đều giống nhau. Nhưng nếu một mảng giới thiệu một giá trị mới sớm hơn hoặc muộn hơn, các dấu hiệu sẽ phân kỳ ngay lập tức. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là so sánh tất cả các giá trị có thể được gắn nhãn lại trong mảng đầu tiên để khớp với mảng thứ hai. Điều này có nghĩa là cố gắng xây dựng một song ánh giữa các giá trị của A và B để bảo toàn vị trí. Đối với mỗi lần ánh xạ, chúng tôi sẽ xác thực tính nhất quán trên các mảng. Số lượng ánh xạ tăng theo giai thừa theo số lượng phần tử riêng biệt, vì bất kỳ hoán vị nào của nhãn đều có thể được xem xét. Ngay cả khi chúng ta hạn chế xây dựng các ánh xạ một cách tham lam, cuối cùng chúng ta vẫn liên tục kiểm tra tính nhất quán trên toàn bộ mảng, điều này làm cho tổng chi phí trở thành bậc hai hoặc tệ hơn. 

Quan sát quan trọng là các giá trị số thực tế không quan trọng chút nào. Điều quan trọng chỉ là liệu chúng ta đã thấy một giá trị trước đó hay chưa và các giá trị mới xuất hiện theo thứ tự nào. Mỗi mảng có thể được rút gọn một cách độc lập thành dạng chuẩn bằng cách thay thế mỗi lần xuất hiện đầu tiên bằng một mã định danh mới và sử dụng lại mã định danh đó cho các lần xuất hiện tiếp theo. 

Khi cả hai mảng được chuyển đổi thành các chuỗi chuẩn này, vấn đề sẽ giảm xuống còn việc kiểm tra tính bằng nhau trực tiếp. 

Điều này có tác dụng vì mối quan hệ “tương tự” được mô tả trong bài toán chính xác là sự đẳng cấu giữa các cấu trúc gây ra bởi lần xuất hiện đầu tiên. Mã hóa chuẩn loại bỏ sự cần thiết phải suy luận về bất kỳ sự tương ứng giá trị thực tế nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lập bản đồ lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Mã hóa Canonical | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng mảng một cách độc lập và xây dựng chữ ký cấu trúc của nó.

1. Tạo một từ điển trống sẽ ánh xạ từng giá trị chưa thấy trước đó thành một mã định danh số nguyên duy nhất. Mã định danh này thể hiện thứ tự mà các giá trị được gặp lần đầu tiên. 
2. Duy trì bộ đếm bắt đầu từ số 0. Bộ đếm này gán các mã định danh mới cho các giá trị mới theo thứ tự tăng dần. 
3. Quét mảng từ trái sang phải. Đối với mỗi phần tử, hãy kiểm tra xem nó đã được nhìn thấy trước đó chưa. 
4. Nếu phần tử là mới, gán cho nó giá trị bộ đếm hiện tại và tăng bộ đếm. Điều này đảm bảo rằng những khám phá trước đó nhận được số nhận dạng nhỏ hơn, duy trì thứ tự khám phá. 
5. Nếu phần tử đã được nhìn thấy, hãy sử dụng lại mã định danh được chỉ định của nó. 
6. Xây dựng một mảng được chuyển đổi bao gồm các mã định danh này. 
7. Lặp lại quy trình tương tự cho mảng thứ hai. 
8. So sánh trực tiếp hai mảng được chuyển đổi. Nếu chúng giống nhau thì xuất ra “YES”, nếu không thì xuất ra “NO”. 

Lý do cấu trúc này đúng là vì việc ánh xạ chỉ phụ thuộc vào chuỗi lần xuất hiện đầu tiên, đây là thông tin cấu trúc duy nhất được bảo toàn dưới bất kỳ phép biến đổi tương tự hợp lệ nào. Bất kỳ hai mảng nào giống nhau đều phải tạo ra các chuỗi khám phá giống hệt nhau, vì lần thứ i một giá trị mới xuất hiện phải tương ứng với giá trị mới thứ i trong mảng kia. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def encode(arr):
    mp = {}
    cur = 0
    res = []
    for x in arr:
        if x not in mp:
            mp[x] = cur
            cur += 1
        res.append(mp[x])
    return res

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    if encode(a) == encode(b):
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp được chia thành chức năng trợ giúp và logic chính. Hàm trợ giúp xây dựng mã hóa chuẩn bằng cách theo dõi lần xuất hiện đầu tiên bằng từ điển. Thứ tự gán là rất quan trọng vì nó buộc rằng lần đầu tiên chúng ta nhìn thấy một giá trị sẽ xác định danh tính của nó. 

Hàm chính đọc cả hai mảng, mã hóa chúng một cách độc lập và so sánh kết quả. Không cần chuẩn hóa bổ sung vì mã hóa đã loại bỏ sự phụ thuộc vào giá trị thô. 

Một chi tiết tinh tế là chúng ta không được sử dụng lại trạng thái giữa hai bảng mã. Mỗi mảng phải bắt đầu bằng một ánh xạ mới để cấu trúc của chúng có thể so sánh được một cách riêng biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

A = [5, 7, 5] 

B = [1, 2, 1] 

Quá trình mã hóa: 

| Bước | Một phần tử | Một bản đồ | Một mã hóa | Phần tử B | Bản đồ B | B được mã hóa | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 5 | {5:0} | [0] | 1 | {1:0} | [0] | 
| 2 | 7 | {5:0,7:1} | [0,1] | 2 | {1:0,2:1} | [0,1] | 
| 3 | 5 | {5:0,7:1} | [0,1,0] | 1 | {1:0,2:1} | [0,1,0] | 

Cả hai mảng được mã hóa đều khớp nhau, vì vậy đầu ra là CÓ. 

Điều này khẳng định rằng sự bình đẳng chỉ phụ thuộc vào cấu trúc chứ không phụ thuộc vào giá trị thực tế. 

### Ví dụ 2 

đầu vào: 

A = [1, 2, 1, 3] 

B = [4, 5, 5, 6] 

| Bước | Một phần tử | Một bản đồ | Một mã hóa | Phần tử B | Bản đồ B | B được mã hóa | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | {1:0} | [0] | 4 | {4:0} | [0] | 
| 2 | 2 | {1:0,2:1} | [0,1] | 5 | {4:0,5:1} | [0,1] | 
| 3 | 1 | {1:0,2:1} | [0,1,0] | 5 | {4:0,5:1} | [0,1,1] | 
| 4 | 3 | {1:0,2:1,3:2} | [0,1,0,2] | 6 | {4:0,5:1,6:2} | [0,1,1,2] | 

Các chuỗi được mã hóa khác nhau ở vị trí thứ 3 nên đầu ra là KHÔNG. 

Điều này chứng tỏ rằng ngay cả một sai lệch nhỏ trong cấu trúc lặp lại cũng phá vỡ sự tương đồng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi mảng được quét một lần và các phép toán từ điển ở mức trung bình O(1) | 
| Không gian | O(n) | Chúng tôi lưu trữ ánh xạ và mảng được mã hóa | 

Giải pháp dễ dàng phù hợp với các ràng buộc điển hình lên tới 2×10^5 phần tử, vì thuật toán chỉ thực hiện công việc tuyến tính cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def encode(arr):
        mp = {}
        cur = 0
        res = []
        for x in arr:
            if x not in mp:
                mp[x] = cur
                cur += 1
            res.append(mp[x])
        return res

    t = 1
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        out.append("YES" if encode(a) == encode(b) else "NO")
    return "\n".join(out)

# sample-like cases
assert run("3\n5 7 5\n1 2 1\n") == "YES"
assert run("4\n1 2 1 3\n4 5 5 6\n") == "NO"

# custom cases
assert run("1\n1\n1\n") == "YES"
assert run("2\n1 2\n2 1\n") == "YES"
assert run("3\n1 2 3\n1 1 1\n") == "NO"
assert run("5\n1 2 3 2 1\n4 5 6 5 4\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử giống nhau | CÓ | độ chính xác kích thước tối thiểu | 
| hoán đổi các giá trị riêng biệt | CÓ | tính đối xứng khi dán nhãn lại | 
| tất cả đều bằng nhau và khác biệt | KHÔNG | cấu trúc lặp lại không khớp | 
| khớp mẫu palindromic | CÓ | tính nhất quán của cấu trúc lặp đi lặp lại | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi cả hai mảng đều chứa tất cả các giá trị giống nhau. Ví dụ: A = [7, 7, 7] và B = [3, 3, 3]. Mã hóa cho cả hai trở thành [0, 0, 0] vì không có giá trị mới nào xuất hiện sau phần tử đầu tiên. Thuật toán xử lý việc này một cách tự nhiên vì từ điển không bao giờ mở rộng quá một mục nhập. 

Một trường hợp khác là khi các mảng có cùng nhiều tập hợp nhưng cấu trúc khác nhau, chẳng hạn như A = [1, 2, 2, 3] và B = [1, 2, 3, 3]. Mảng đầu tiên mã hóa là [0, 1, 1, 2], trong khi mảng thứ hai trở thành [0, 1, 2, 2]. Sự không khớp xảy ra chính xác ở vị trí thứ ba, nơi mà danh tính của “lần xuất hiện đầu tiên” phân kỳ. 

Trường hợp tinh tế cuối cùng là khi giá trị lớn hoặc âm. Vì chúng tôi chỉ dựa vào hàm băm trong từ điển nên độ lớn thực tế của các giá trị không ảnh hưởng đến tính chính xác hoặc hiệu suất.
