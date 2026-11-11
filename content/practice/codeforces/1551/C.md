---
title: "CF 1551C - Câu Chuyện Thú Vị"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, chúng tôi nhận được một danh sách các từ và chúng tôi muốn chọn càng nhiều từ trong số này càng tốt để tập hợp con được chọn thỏa mãn điều kiện mất cân bằng cụ thể."
date: "2026-06-14T20:41:34+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings", "strings"]
categories: ["algorithms"]
codeforces_contest: 1551
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 734 (Div. 3)"
rating: 1500
weight: 1551
solve_time_s: 287
verified: true
draft: false
---

[CF 1551C - Câu chuyện thú vị](https://codeforces.com/problemset/problem/1551/C) 

**Đánh giá:** 1500 
**Tags:** tham lam, sắp xếp, chuỗi 
**Thời gian giải:** 4 phút 47 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, chúng tôi nhận được một danh sách các từ và chúng tôi muốn chọn càng nhiều từ trong số này càng tốt để tập hợp con được chọn thỏa mãn điều kiện mất cân bằng cụ thể. 

Điều kiện được đánh giá bằng cách xem xét tần số chữ cái trên tất cả các từ đã chọn. Trong số các chữ cái`a, b, c, d, e`, chúng ta muốn tìm một chữ cái sao cho tổng số lần xuất hiện của nó trong các từ đã chọn lớn hơn tổng số lần xuất hiện của tất cả các chữ cái khác cộng lại. 

Một cách khác để thấy điều này là hãy tưởng tượng chúng ta chọn một số từ, sau đó đếm xem mỗi chữ cái xuất hiện bao nhiêu lần trên toàn cầu. Chúng tôi đang tìm kiếm một chữ cái “thống trị” phần còn lại của bảng chữ cái cộng lại. 

Nhiệm vụ không phải là tối đa hóa bất kỳ thứ gì bên trong một từ mà là chọn một tập hợp con các từ có thể tạo ra sự thống trị này, đồng thời giữ cho tập hợp con đó càng lớn càng tốt. 

Các ràng buộc rất lớn: tổng số từ trong tất cả các trường hợp thử nghiệm lên tới 2 · 10^5 và tổng chiều dài ký tự lên tới 4 · 10^5. Điều này loại trừ bất kỳ giải pháp nào thử tất cả các tập hợp con hoặc tính toán lại nhiều lần số lượng ký tự cho mỗi tập hợp con ứng cử viên. Mọi giải pháp cho mỗi trường hợp thử nghiệm đều phải gần với tuyến tính hoặc tuyến tính trong tổng kích thước đầu vào. 

Một ý tưởng ngây thơ là thử mọi tập hợp con các từ, nhưng đó là hàm mũ theo n và ngay lập tức không khả thi. Ngay cả việc cố gắng sắp xếp các tập hợp con hoặc tính toán lại các bảng tần số đầy đủ cho mỗi tiền tố cũng sẽ quá chậm. 

Một kiểu thất bại tinh tế hơn xuất phát từ việc tham lam lấy tất cả các từ: điều này rõ ràng là thất bại khi không có một chữ cái nào chiếm ưu thế trên toàn cầu. Ví dụ: những từ cân bằng như`"abcde"`đóng góp như nhau cho tất cả các chữ cái, khiến cho việc chiếm ưu thế là không thể dù có lấy bao nhiêu chữ cái. 

Một vấn đề tế nhị khác là giả sử chúng ta nên chọn những từ có chữ cái được chọn thường xuyên xuất hiện bên trong từ đó. Chỉ điều đó thôi là chưa đủ vì điều kiện phụ thuộc vào _so sánh tổng thể với tất cả các chữ cái khác cộng lại_, chứ không chỉ tối đa hóa một chữ cái cục bộ. 

## Phương pháp tiếp cận 

Khó khăn chính là điều kiện mang tính tổng thể đối với số lượng chữ cái, nhưng các quyết định được đưa ra ở cấp độ từ. Điều này gợi ý việc giảm từng từ thành mức độ nó “giúp đỡ” hoặc “gây tổn hại” cho lá thư của ứng viên. 

Sửa một chữ cái, nói`x`. Đối với một tập hợp các từ đã chọn, chúng tôi muốn:```
count_x > sum of counts of all other letters
```Viết lại điều này, chúng tôi xác định cho mỗi từ một điểm tương ứng với`x`:```
score(word, x) = (# of x in word) - (# of non-x letters in word)
```Nếu chúng ta tính tổng số này trên các từ đã chọn, điều kiện sẽ trở thành:```
total_score(x) > 0
```Vì vậy, đối với một chữ cái cố định, mỗi từ sẽ trở thành một số nguyên đóng góp duy nhất. Chúng tôi muốn chọn càng nhiều từ càng tốt sao cho tổng số điểm của chúng là dương. Chiến lược tối ưu cho một chữ cái cố định là tham lam: sắp xếp các từ theo điểm theo thứ tự giảm dần và lấy tiền tố trong khi tổng số vẫn dương. 

Câu trả lời cuối cùng là kết quả tốt nhất trên cả 5 chữ cái. 

Điều này có tác dụng vì sau khi chúng tôi sửa chữ cái đích, vấn đề sẽ trở thành lựa chọn tiền tố có kích thước tối đa dưới một ràng buộc đơn điệu về tổng tiền tố. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Hãy thử từng chữ cái + sắp xếp tham lam | O(5 · n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đối với mỗi chữ cái trong năm chữ cái`a`ĐẾN`e`, treat it as the candidate dominant letter.

 Điều này làm giảm vấn đề kiểm tra xem chữ cái đó có thể thống trị phần còn lại hay không. 
2. Với mỗi từ, hãy tính điểm đóng góp của nó cho chữ cái đã chọn: đếm số lần xuất hiện của chữ cái đó trừ đi số lần xuất hiện của tất cả các chữ cái khác. 

Điều này chuyển đổi mỗi từ thành một giá trị số nguyên duy nhất thể hiện tính hữu dụng của nó đối với chữ cái này. 
3. Sắp xếp tất cả các từ theo số điểm này theo thứ tự giảm dần. 

Điều này đảm bảo chúng tôi xem xét các từ có lợi nhất trước tiên, giúp tối đa hóa thời gian duy trì tiền tố dương. 
4. Duyệt qua danh sách đã sắp xếp và duy trì tổng hiện có. Tiếp tục mở rộng tiền tố miễn là việc thêm từ tiếp theo giữ cho tổng hoàn toàn dương. 

Once the sum would become non-positive, we stop.
 5. Ghi lại số từ tối đa có thể đạt được cho bức thư này. 
6. Lặp lại cho tất cả năm chữ cái và lấy kết quả tối đa trên chúng. 

### Tại sao nó hoạt động 

Đối với một chữ cái cố định, phép biến đổi làm giảm ràng buộc ban đầu thành bất đẳng thức tuyến tính trên tổng các đóng góp độc lập. Bất kỳ lựa chọn tối ưu nào trước tiên đều phải bao gồm những từ có lợi ích cận biên cao nhất; mặt khác, việc hoán đổi một từ có điểm thấp hơn bằng một từ có điểm cao hơn chỉ làm tăng tổng số tiền hoặc giữ nguyên tổng số trong khi không làm giảm kích thước. Đối số trao đổi này đảm bảo rằng tập hợp con hợp lệ tốt nhất tương ứng với tiền tố của thứ tự được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(words):
    best = 0

    for target in "abcde":
        vals = []
        for w in words:
            c = 0
            for ch in w:
                if ch == target:
                    c += 1
                else:
                    c -= 1
            vals.append(c)

        vals.sort(reverse=True)

        s = 0
        cnt = 0
        for v in vals:
            if s + v <= 0:
                break
            s += v
            cnt += 1

        best = max(best, cnt)

    return best

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        words = [input().strip() for _ in range(n)]
        print(solve_case(words))

if __name__ == "__main__":
    main()
```Mã tuân theo mức giảm được mô tả trước đó. Mỗi từ được chuyển thành điểm tương ứng với chữ cái đích hiện tại. Việc sắp xếp đảm bảo chúng tôi luôn ưu tiên những từ hỗ trợ mạnh mẽ nhất cho điều kiện thống trị. Việc tích lũy tiền tố trực tiếp thực thi yêu cầu tích cực nghiêm ngặt. 

Một chi tiết tinh tế là điều kiện dừng`s + v <= 0`. Chúng ta phải dừng ngay lập tức khi việc thêm một từ sẽ phá vỡ sự thống trị nghiêm ngặt, vì bất kỳ từ nào khác cũng không tốt hơn do thứ tự sắp xếp. Một điểm quan trọng khác là tính lại điểm riêng cho từng chữ cái, vì tập hợp con tối ưu phụ thuộc rất nhiều vào chữ cái nào được chọn làm chữ cái chiếm ưu thế. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
bac
aaada
e
```Chúng tôi đánh giá thư`a`: 

| Bước | Lời | Điểm | Tổng Chạy | Đã chụp | 
| --- | --- | --- | --- | --- | 
| 1 | aaa | +3 | 3 | vâng | 
| 2 | bac | -1 | 2 | vâng | 
| 3 | e | -1 | 1 | vâng | 

Chúng tôi đã thực hiện thành công cả 3 từ. 

Điều này hoạt động vì`a`thống trị phần còn lại sau khi tổng hợp. 

### Ví dụ 2 

đầu vào:```
3
aba
abcde
aba
```Đối với thư`a`: 

| Bước | Lời | Điểm | Tổng Chạy | Đã chụp | 
| --- | --- | --- | --- | --- | 
| 1 | aba | +1 | 1 | vâng | 
| 2 | aba | +1 | 2 | vâng | 
| 3 | abcde | -3 | - | dừng lại | 

Chúng tôi dừng lại trước khi lấy tất cả các từ, vì bao gồm`abcde`phá vỡ sự thống trị. 

Điều này chứng tỏ tại sao việc sắp xếp tham lam là cần thiết: một từ “xấu” trên toàn cầu phải bị trì hoãn hoặc loại trừ ngay cả khi nó xuất hiện sớm trong đầu vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(5 · n log n) | Mỗi bài kiểm tra sắp xếp n điểm từ cho mỗi 5 chữ cái | 
| Không gian | O(n) | Lưu trữ điểm chuyển đổi theo từng từ | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm bị giới hạn, do đó giải pháp chạy thoải mái trong giới hạn. Việc sắp xếp chiếm ưu thế trong thời gian chạy nhưng vẫn hiệu quả với tổng số 2 · 10^5 từ. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve_case(words):
        best = 0
        for target in "abcde":
            vals = []
            for w in words:
                c = 0
                for ch in w:
                    c += 1 if ch == target else -1
                vals.append(c)
            vals.sort(reverse=True)
            s = 0
            cnt = 0
            for v in vals:
                if s + v <= 0:
                    break
                s += v
                cnt += 1
            best = max(best, cnt)
        return best

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        words = [input().strip() for _ in range(n)]
        out.append(str(solve_case(words)))
    return "\n".join(out)

# provided samples
assert solve("""6
3
bac
aaada
e
3
aba
abcde
aba
2
baba
baba
4
ab
ab
c
bc
5
cbdca
d
a
d
e
3
b
c
ca
""") == """3
2
0
2
3
2"""

# edge cases
assert solve("""1
1
abcde
""") == "0"

assert solve("""1
2
a
a
""") == "2"

assert solve("""1
3
abc
abc
abc
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| từ cân bằng duy nhất | 0 | không thể có sự thống trị | 
| tất cả các từ có một chữ cái giống hệt nhau | 2 | tham lam chọn lọc đầy đủ | 
| từ đầy đủ đối xứng | 0 | tất cả các chữ cái hủy bỏ | 

## Vỏ cạnh 

Trường hợp quan trọng là khi mỗi từ được cân bằng hoàn hảo như`"abcde"`. Đối với đầu vào như vậy, mọi chữ cái đều đóng góp như nhau nên mọi điểm đều bằng 0. Thuật toán sắp xếp các số 0 và dừng ngay lập tức vì phép cộng đầu tiên đã vi phạm tính tích cực nghiêm ngặt. 

Một trường hợp đặc biệt khác là khi tất cả các từ bao gồm một chữ cái lặp lại, chẳng hạn`"a", "a", "a"`. Đối với thư mục tiêu`a`, mọi điểm đều dương nên tất cả các từ đều được lấy. Đối với các chữ cái khác, tất cả điểm đều âm, không tạo ra lựa chọn nào. Số tối đa chính xác sẽ trở thành số lượng đầy đủ. 

Trường hợp thứ ba là sự thống trị hỗn hợp trong đó một từ hỗ trợ mạnh mẽ cho một chữ cái nhưng lại gây hại đôi chút cho nó về tổng thể, tạo ra các hiệu ứng trật tự. Bước sắp xếp đảm bảo các từ như vậy được đặt chính xác và quy tắc tiền tố sẽ ngăn các lựa chọn không hợp lệ xuất hiện sau này trong chuỗi.
