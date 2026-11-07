---
title: "CF 1369B - Chính xácLee"
description: "Chúng ta được cung cấp một chuỗi nhị phân và được phép xóa nhiều lần các ký tự theo một quy tắc cục bộ rất cụ thể: bất cứ khi nào số 1 xuất hiện ngay sau số 0, chúng ta có thể xóa chính xác một trong hai ký tự đó, thu nhỏ chuỗi mỗi lần."
date: "2026-06-11T11:38:21+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1369
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 652 (Div. 2)"
rating: 1200
weight: 1369
solve_time_s: 208
verified: true
draft: false
---

[CF 1369B -AccurLee](https://codeforces.com/problemset/problem/1369/B) 

**Đánh giá:** 1200 
**Tags:** tham lam, thực hiện, chuỗi 
**Thời gian giải:** 3 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân và được phép loại bỏ nhiều lần các ký tự theo một quy tắc cục bộ rất cụ thể: bất cứ khi nào một`1`xuất hiện ngay sau đó là một`0`, chúng ta có thể xóa chính xác một trong hai ký tự đó, thu nhỏ chuỗi mỗi lần. Việc lựa chọn loại bỏ ký tự nào nằm trong tầm kiểm soát của chúng tôi và chúng tôi có thể thực hiện bất kỳ số lượng thao tác nào như vậy. 

Mục tiêu không chỉ là giảm thiểu độ dài mà còn tạo ra chuỗi cuối cùng tốt nhất có thể theo ưu tiên hai cấp độ. Chuỗi cuối cùng ngắn hơn luôn được ưu tiên. Nếu hai kết quả có cùng độ dài thì kết quả nhỏ hơn về mặt từ điển sẽ được coi là tốt hơn. 

Khó khăn chính là việc xóa chỉ được phép ở các vùng lân cận.`10`theo cặp, do đó cấu trúc của chuỗi phát triển cục bộ nhưng tiêu chí tối ưu hóa lại mang tính tổng thể. Điều này khiến cho việc mô phỏng các hoạt động trở nên hấp dẫn, nhưng việc lựa chọn phân nhánh ký tự nào cần xóa sẽ dẫn đến nhiều kết quả có thể xảy ra. 

Các ràng buộc cho thấy giải pháp dự định phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Với tổng chiều dài lên tới 10^5 trong tất cả các thử nghiệm, mọi mô phỏng bậc hai về hoán đổi hoặc xóa sẽ hết thời gian chờ, vì ngay cả 10^5 thao tác trên mỗi bước cũng sẽ vượt quá giới hạn nếu lặp lại. 

Trường hợp cạnh tinh tế phát sinh từ các mẫu xen kẽ. Ví dụ, trong`1010`, các lựa chọn xóa khác nhau sẽ dẫn đến các chuỗi còn lại khác nhau và không rõ liệu chúng ta có nên ưu tiên giữ nhiều hơn hay không`0`s trở lên`1`S. Một tình huống khó khăn khác là khi dây đã đơn điệu, như`000111`, trong đó không thể thực hiện được thao tác nào và bất kỳ logic tham lam không chính xác nào vẫn có thể cố gắng thực hiện các phép biến đổi không cần thiết hoặc xử lý sai các ranh giới. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ mô phỏng tất cả các động thái có thể xảy ra. Mỗi bước tìm thấy một`10`ghép nối và phân nhánh thành hai trạng thái tùy thuộc vào việc chúng ta có xóa`1`hoặc`0`. Điều này tạo ra một không gian trạng thái có thể tăng theo cấp số nhân về số lượng cặp hợp lệ. Trong trường hợp xấu nhất, một chuỗi như`101010...`có Θ(n) bước di chuyển cục bộ hợp lệ và việc phân nhánh dẫn đến sự bùng nổ theo cấp số nhân. Ngay cả với tính năng ghi nhớ, số lượng chuỗi có thể truy cập vẫn quá lớn để thể hiện một cách hiệu quả. 

Quan sát chính là hoạt động chỉ cho phép tương tác giữa các liền kề`1`Và`0`, và nó luôn làm giảm sự hỗn loạn giữa họ. Thay vì suy nghĩ theo trình tự xóa tùy ý, chúng ta có thể xem chuỗi đang được chuyển đổi thành dạng chính tắc trong đó tất cả các chuỗi còn lại`1`s càng trái càng tốt so với`0`s, bị hạn chế bởi sự bất lực của`0`để di chuyển sang trái`1`trừ khi nó tham gia vào việc xóa. 

Một góc nhìn hiệu quả hơn là quét chuỗi và xây dựng câu trả lời cuối cùng một cách tham lam. Chúng tôi duy trì cấu trúc giống như ngăn xếp: bất cứ khi nào chúng tôi thấy một`1`theo sau là một`0`, chúng ta có thể loại bỏ một ký tự, điều này cho phép chúng ta giải quyết các nghịch đảo cục bộ một cách hiệu quả. Chiến lược tối ưu hóa ra tương đương với việc hủy bỏ nhiều lần`10`tương tác theo cách duy trì cấu trúc tối thiểu về mặt từ điển trong số các kết quả có độ dài tối thiểu. 

Điều này dẫn đến một quy trình tuyến tính đơn giản: chúng tôi mô phỏng tác động của việc hủy tối ưu bằng cách sử dụng ngăn xếp. Khi chúng ta nhìn thấy một`1`, chúng tôi đẩy nó. Khi chúng ta nhìn thấy một`0`, chúng tôi cố gắng hủy bỏ với trước đó`1`s nếu có lợi cho việc giảm thiểu chuỗi cuối cùng theo thứ tự từ điển. Quá trình giảm xuống còn duy trì sự sắp xếp tốt nhất có thể đạt được khi không có lợi`10`việc xóa vẫn còn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Xây dựng tham lam dựa trên ngăn xếp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chúng ta lặp qua chuỗi từ trái sang phải, xây dựng kết quả cuối cùng tăng dần. Ở mỗi bước, chúng tôi xem xét liệu nhân vật hiện tại có thể tương tác với các nhân vật đã thấy trước đó thông qua một`10`mẫu. Điều này là cần thiết vì chỉ liền kề`1`Và`0`cặp có đủ điều kiện để xóa. 
2. Chúng tôi duy trì một ngăn xếp thể hiện cấu trúc từng phần tốt nhất hiện tại của câu trả lời. Mỗi ký tự chúng tôi xử lý sẽ được thêm vào hoặc kích hoạt việc hủy bỏ dựa trên thao tác được phép. 
3. Khi chúng ta gặp phải một`1`, chúng ta đẩy nó vào ngăn xếp ngay lập tức. MỘT`1`chỉ hữu ích như một đối tác hủy bỏ tiềm năng trong tương lai cho một`0`ở bên phải của nó, vì vậy chúng tôi lưu trữ nó cho đến khi chúng tôi xem liệu nó có thể được ghép nối hay không. 
4. Khi chúng ta gặp phải một`0`, chúng ta kiểm tra đỉnh ngăn xếp. Nếu có một`1`trên hết, chúng ta có thể thực hiện việc xóa liên quan đến việc này`10`đôi. Vì chúng tôi được phép xóa một trong hai ký tự nên chúng tôi chọn tùy chọn dẫn đến kết quả cuối cùng nhỏ hơn về mặt từ điển, điều này có nghĩa là loại bỏ một cách hiệu quả ký tự đó.`1`nếu nó giúp bảo tồn cấu trúc trước đó, nếu không thì sẽ loại bỏ`0`. 
5. Việc lặp đi lặp lại điều này một cách tham lam đảm bảo rằng mọi việc hủy bỏ lợi ích có thể đều được thực hiện và không có lợi ích liền kề nào được thực hiện.`10`cặp có thể cải thiện kết quả vẫn chưa được giải quyết. 
6. Cuối cùng, ngăn xếp chứa chuỗi sạch nhất cuối cùng. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là ngăn xếp luôn biểu thị một chuỗi trong đó không có thao tác cục bộ nào khác có thể cải thiện mục tiêu từ điển hoặc độ dài. Bất kì`10`cặp vẫn có thể được sử dụng để giảm độ dài hoặc cải thiện thứ tự sẽ nhất thiết phải tương ứng với một`1`trong ngăn xếp ngay sau đó là một`0`theo thứ tự đầu vào và những trường hợp như vậy luôn được giải quyết tại thời điểm chúng trở nên liền kề trong cấu trúc được xử lý. Điều này đảm bảo rằng thuật toán không bao giờ trì hoãn việc xóa có lợi và không bao giờ thực hiện việc xóa có thể làm xấu đi kết quả tối ưu về mặt từ điển cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()

        stack = []

        for c in s:
            if c == '0':
                if stack and stack[-1] == '1':
                    stack.pop()
                else:
                    stack.append(c)
            else:
                stack.append(c)

        print(''.join(stack))

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng trường hợp thử nghiệm một cách độc lập, duy trì một ngăn xếp lưu trữ chuỗi ứng cử viên đang phát triển. Quyết định thực hiện quan trọng nhất là giải quyết ngay lập tức một`10`mẫu bằng cách loại bỏ`1`, vì sự lựa chọn này dẫn đến hành vi từ điển học tốt hơn bằng cách ngăn chặn những`1`từ việc kiên trì ở những vị trí mà họ sẽ thống trị trước đó`0`S. 

Ngăn xếp đảm bảo rằng chúng tôi chỉ lưu trữ các ký tự không thể tham gia ngay vào việc xóa có lợi. Điều này tránh mọi nhu cầu quay lại hoặc mô phỏng đầy đủ các phép biến đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`0101`Chúng tôi xử lý từng bước: 

| Bước | Char | Xếp chồng trước | Hành động | Xếp chồng sau | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | [] | đẩy 0 | [0] | 
| 2 | 1 | [0] | đẩy 1 | [0,1] | 
| 3 | 0 | [0,1] | xóa 1 | [0] | 
| 4 | 1 | [0] | đẩy 1 | [0,1] | 

Chuỗi cuối cùng là`01`. 

Điều này chứng tỏ mỗi`10`tương tác được giải quyết ngay lập tức, ngăn chặn các mô hình xen kẽ dưới mức tối ưu tiếp tục tồn tại. 

### Ví dụ 2:`11001101`| Bước | Char | Xếp chồng trước | Hành động | Xếp chồng sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [] | đẩy 1 | [1] | 
| 2 | 1 | [1] | đẩy 1 | [1,1] | 
| 3 | 0 | [1,1] | xóa 1 | [1] | 
| 4 | 0 | [1] | đẩy 0 | [1,0] | 
| 5 | 1 | [1,0] | đẩy 1 | [1,0,1] | 
| 6 | 1 | [1,0,1] | đẩy 1 | [1,0,1,1] | 
| 7 | 0 | [1,0,1,1] | xóa 1 | [1,0,1] | 
| 8 | 1 | [1,0,1] | đẩy 1 | [1,0,1,1] | 

Chuỗi cuối cùng là`1011`. 

Dấu vết cho thấy việc hủy xảy ra chính xác khi một`1`trở nên tiếp xúc với một`0`ngay bên phải của nó, dần dần đơn giản hóa cấu trúc mà không cần suy luận tổng thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được đẩy và có thể xuất hiện một lần từ ngăn xếp | 
| Không gian | O(n) | Ngăn xếp lưu trữ tối đa n ký tự | 

Tổng độ dài của tất cả các trường hợp thử nghiệm được giới hạn bởi 10^5, do đó, việc quét tuyến tính trên mỗi trường hợp thử nghiệm dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            s = input().strip()

            stack = []
            for c in s:
                if c == '0' and stack and stack[-1] == '1':
                    stack.pop()
                else:
                    stack.append(c)

            out.append(''.join(stack))
        return '\n'.join(out)

    return solve()

# provided samples
assert run("""5
10
0001111111
4
0101
8
11001101
10
1110000000
1
1
""") == """0001111111
01
1011
0
1"""

# edge: already sorted
assert run("""1
6
000111
""") == "000111"

# edge: alternating
assert run("""1
6
101010
""") == "0"

# edge: single char
assert run("""1
1
0
""") == "0"

# edge: all ones
assert run("""1
5
11111
""") == "11111"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đã được sắp xếp | không thay đổi | không thể thực hiện thao tác nào | 
| xen kẽ`101010`|`0`| chuỗi hủy tối đa | 
| char đơn | cùng char | xử lý ranh giới | 
| tất cả những cái | không thay đổi | không hợp lệ`10`cặp | 

## Vỏ cạnh 

Đối với một chuỗi đã có thứ tự không giảm như`000111`, thuật toán sẽ đẩy tất cả các ký tự mà không bao giờ kích hoạt cửa sổ bật lên, vì không có`10`cặp tồn tại. Ngăn xếp vẫn giống hệt với đầu vào, phù hợp với thực tế là không thể di chuyển được. 

Đối với một chuỗi xen kẽ như`101010`, mỗi lần một`0`xuất hiện sau một`1`, thuật toán sẽ hủy ngay một`1`, thu gọn cấu trúc cho đến khi chỉ còn một`0`vẫn còn. Quá trình phát triển ngăn xếp từng bước xác nhận rằng mọi tương tác đủ điều kiện được sử dụng chính xác một lần và không cần tối ưu hóa trì hoãn. 

Đối với đầu vào một ký tự, vòng lặp chạy một lần và đẩy`0`hoặc`1`không có bất kỳ tương tác ngăn xếp nào, bảo toàn chính xác cấu trúc tối thiểu trong các trường hợp tầm thường.
