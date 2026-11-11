---
title: "CF 1552A - Hoán vị chuỗi sau"
description: "Chúng ta được cấp một chuỗi gồm các chữ cái viết thường. Trong một lần di chuyển, chúng ta được phép chọn chính xác một tập hợp con các vị trí, lấy các ký tự ở các vị trí đó và chỉ sắp xếp lại những ký tự đã chọn đó một cách tùy ý trong khi giữ tất cả các ký tự khác cố định ở vị trí ban đầu."
date: "2026-06-14T20:57:38+07:00"
tags: ["codeforces", "competitive-programming", "sortings", "strings"]
categories: ["algorithms"]
codeforces_contest: 1552
codeforces_index: "A"
codeforces_contest_name: "Codeforces Global Round 15"
rating: 800
weight: 1552
solve_time_s: 382
verified: true
draft: false
---

[CF 1552A - Hoán vị chuỗi sau](https://codeforces.com/problemset/problem/1552/A) 

**Đánh giá:** 800 
**Thẻ:** sắp xếp, chuỗi 
**Thời gian giải:** 6 phút 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi gồm các chữ cái viết thường. Trong một lần di chuyển, chúng ta được phép chọn chính xác một tập hợp con các vị trí, lấy các ký tự ở các vị trí đó và chỉ sắp xếp lại những ký tự đã chọn đó một cách tùy ý trong khi giữ tất cả các ký tự khác cố định ở vị trí ban đầu. 

Mục đích là làm cho toàn bộ chuỗi được sắp xếp theo thứ tự bảng chữ cái không giảm sau đúng một thao tác như vậy. Chúng tôi được yêu cầu giảm thiểu số lượng ký tự chúng tôi cần chọn cho thao tác này. 

Một cách hữu ích để suy nghĩ về quy trình là chuỗi cuối cùng phải khớp với phiên bản được sắp xếp của chuỗi gốc. Chúng tôi không chèn hoặc xóa các ký tự, chỉ di chuyển một tập hợp con đã chọn giữa chúng trong khi các ký tự còn lại hoạt động như những cái neo cố định. 

Các ràng buộc rất nhỏ: độ dài chuỗi tối đa là 40 và có tối đa 1000 trường hợp thử nghiệm. Điều này ngay lập tức gợi ý rằng các giải pháp lên tới khoảng O(n2) hoặc thậm chí O(n³) cho mỗi trường hợp thử nghiệm là an toàn, nhưng bất kỳ số mũ nào trong 40 vẫn sẽ là giới hạn trừ khi được cấu trúc cẩn thận. Tuy nhiên, cấu trúc thực sự của bài toán cho phép giải pháp quét tuyến tính. 

Một vấn đề tế nhị là tập hợp con được chọn có thể ở bất kỳ vị trí nào, không nhất thiết phải liền kề nhau. Điều này có nghĩa là những lựa chọn tham lam ngây thơ như “sửa lỗi đảo ngược cục bộ” có thể thất bại, bởi vì một ký tự bị đặt sai vị trí có thể sửa được bằng cách tham gia vào quá trình sắp xếp lại toàn bộ tập hợp đã chọn. 

Một cái bẫy tiềm ẩn khác là giả định rằng chúng ta phải khớp vị trí chuỗi đã sắp xếp theo từng vị trí với sự không khớp. Điều đó gần đúng với sự thật, nhưng vẫn chưa đủ trừ khi chúng ta giải thích được tại sao chỉ riêng sự không phù hợp đã là đặc điểm của lựa chọn tối ưu. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ thử tất cả các tập hợp con vị trí, mô phỏng việc sắp xếp lại các ký tự đã chọn và kiểm tra xem chuỗi kết quả có thể được sắp xếp hay không. Đối với một chuỗi có độ dài n, điều này có nghĩa là lặp lại trên 2ⁿ tập hợp con. Đối với mỗi tập hợp con, chúng tôi sẽ trích xuất các ký tự, sắp xếp chúng và đặt chúng trở lại, sau đó xác minh xem chuỗi cuối cùng có được sắp xếp hay không. Ngay cả khi việc kiểm tra là O(n log n), vụ nổ tập hợp con vẫn chiếm ưu thế. Với n lên tới 40 thì 2⁴⁰ là hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta thực sự không cần mô phỏng các tập hợp con. Chuỗi đích cuối cùng được cố định: đó là phiên bản được sắp xếp của chuỗi gốc. Câu hỏi duy nhất là vị trí nào đã phù hợp với mục tiêu này mà không bị di chuyển. 

Nếu một ký tự ở vị trí i đã bằng ký tự sẽ xuất hiện ở đó trong chuỗi đã sắp xếp, chúng ta có tùy chọn để nguyên ký tự đó. Bất kỳ vị trí nào không khớp đều phải tham gia vào tập hợp con đã chọn, nếu không thì không bao giờ có thể sửa được, vì các vị trí không thay đổi sẽ bảo toàn các ký tự của chúng. 

Điều này biến vấn đề thành sự so sánh giữa chuỗi gốc và phiên bản được sắp xếp của nó. Số lượng thay đổi tối thiểu cần thiết chính xác là số vị trí mà chúng khác nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ · n log n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng dạng đã sắp xếp của chuỗi. Điều này thể hiện sự sắp xếp mục tiêu chính xác mà chúng tôi muốn đạt được sau hoạt động. 
2. So sánh chuỗi gốc với phiên bản đã sắp xếp này theo từng ký tự. 
3. Đếm xem hai chuỗi có bao nhiêu vị trí khác nhau. 
4. Xuất số liệu này ra làm câu trả lời. 

Mỗi vị trí khác nhau đại diện cho một ký tự chưa ở đúng vị trí cuối cùng. Vì các vị trí không thay đổi vẫn cố định và không thể thay đổi gián tiếp nên mọi sự không khớp phải được đưa vào tập con đã chọn ít nhất một lần. 

## Tại sao nó hoạt động

Sửa chuỗi đã sắp xếp làm cấu hình cuối cùng. Bất kỳ vị trí nào mà chuỗi gốc đã khớp với cấu hình này đều không cần tham gia vào thao tác. Những vị trí đó có thể được loại trừ một cách an toàn vì việc giữ nguyên chúng sẽ duy trì tính chính xác. 

Bây giờ hãy xem xét vị trí mà chuỗi gốc và chuỗi được sắp xếp khác nhau. Nếu vị trí đó không được chọn, ký tự của nó vẫn cố định và không thể di chuyển đi nơi khác, do đó chuỗi cuối cùng không thể khớp với mục tiêu đã sắp xếp. Do đó, mọi vị trí không khớp đều bị buộc phải đưa vào tập hợp đã chọn. 

Ngược lại, việc chọn tất cả các vị trí không khớp là đủ vì các ký tự còn lại đã ở đúng thứ tự tương đối đối với các vị trí cố định và các ký tự đã chọn có thể được hoán vị để lấp đầy các vị trí còn lại theo thứ tự đã sắp xếp. 

Điều này tạo ra sự tương đương chính xác: số ký tự được chọn tối thiểu bằng số vị trí không khớp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        
        target = ''.join(sorted(s))
        
        k = 0
        for i in range(n):
            if s[i] != target[i]:
                k += 1
        
        print(k)

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng phiên bản chuỗi đã sắp xếp một lần cho mỗi trường hợp thử nghiệm, sau đó thực hiện quét tuyến tính để đếm các chuỗi không khớp. Chi tiết triển khai quan trọng là việc sắp xếp tạo ra sự sắp xếp cuối cùng chính xác mà chúng ta phải đạt được, do đó không cần mô phỏng hoạt động được phép. 

Câu trả lời không phải là “số lần hoán đổi” hay “đảo ngược”, mà chính xác là số lượng vị trí đã không đồng ý với cấu hình được sắp xếp cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`lol`Chúng tôi tính toán chuỗi được sắp xếp mục tiêu. 

| tôi | s[i] | mục tiêu[i] | trận đấu | 
| --- | --- | --- | --- | 
| 0 | tôi | tôi | vâng | 
| 1 | o | tôi | không | 
| 2 | tôi | o | không | 

Số lượng không khớp là 2, vì vậy k = 2. 

Điều này cho thấy rằng mặc dù về mặt khái niệm chỉ tồn tại một sự đảo ngược nhưng phải có hai vị trí tham gia vì cả hai ký tự đặt sai vị trí đều phải được sắp xếp lại. 

### Ví dụ 2:`dcba`| tôi | s[i] | mục tiêu[i] | trận đấu | 
| --- | --- | --- | --- | 
| 0 | d | một | không | 
| 1 | c | b | không | 
| 2 | b | c | không | 
| 3 | một | d | không | 

Tất cả các vị trí đều khác nhau nên k = 4. 

Điều này xác nhận trường hợp cực đoan khi không có ký tự nào được căn chỉnh với vị trí được sắp xếp của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t · n log n) | sắp xếp từng chuỗi chiếm ưu thế trên mỗi trường hợp thử nghiệm | 
| Không gian | O(n) | lưu trữ chuỗi đã sắp xếp | 

Các ràng buộc cho phép tối đa 1000 chuỗi có độ dài tối đa là 40, do đó việc sắp xếp từng chuỗi là chuyện nhỏ trong giới hạn. Tổng số hoạt động vẫn thấp hơn nhiều so với bất kỳ ngưỡng giới hạn thời gian nào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    output = []

    t = int(sys.stdin.readline())
    for _ in range(t):
        n = int(sys.stdin.readline())
        s = sys.stdin.readline().strip()
        target = ''.join(sorted(s))
        k = sum(1 for i in range(n) if s[i] != target[i])
        output.append(str(k))

    return "\n".join(output)

# provided samples
assert run("""4
3
lol
10
codeforces
5
aaaaa
4
dcba
""") == """2
6
0
4"""

# custom cases
assert run("""1
1
a
""") == "0"

assert run("""1
2
ba
""") == "2"

assert run("""1
6
abcdef
""") == "0"

assert run("""1
5
edcba
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ký tự đơn | 0 | ranh giới tối thiểu | 
| cặp đảo ngược | 2 | sự cần thiết trao đổi không tầm thường nhỏ nhất | 
| đã được sắp xếp | 0 | không cần thao tác | 
| đảo ngược hoàn toàn | n-1 không khớp | trường hợp rối loạn tối đa | 

## Vỏ cạnh 

Một chuỗi ký tự đơn như`a`tạo ra cùng một chuỗi sau khi sắp xếp. Thuật toán tính toán phiên bản được sắp xếp là`a`, so sánh vị trí 0, không tìm thấy điểm không khớp và trả về 0. Điều này xác nhận rằng không có lựa chọn không cần thiết nào được thực hiện khi chuỗi đã tối ưu. 

Một chuỗi đảo ngược hoàn toàn như`dcba`bản đồ tới`abcd`. Mỗi chỉ số đều khác nhau nên tất cả các vị trí đều được tính. Thuật toán buộc k = n một cách chính xác, vì không có ký tự nào được căn chỉnh với vị trí cuối cùng của nó và không có ký tự nào có thể cố định mà không ngăn cản việc sắp xếp.
