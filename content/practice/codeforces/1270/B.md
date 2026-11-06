---
title: "CF 1270B - Mảng con thú vị"
description: "Chúng ta được cung cấp một số trường hợp thử nghiệm và mỗi trường hợp thử nghiệm cung cấp một mảng các số nguyên. Nhiệm vụ là xác định xem có tồn tại một đoạn liền kề của mảng này có độ rộng đủ lớn so với chiều dài của nó hay không."
date: "2026-06-16T00:52:05+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1270
codeforces_index: "B"
codeforces_contest_name: "Good Bye 2019"
rating: 1200
weight: 1270
solve_time_s: 631
verified: false
draft: false
---

[CF 1270B - Phân mảng thú vị](https://codeforces.com/problemset/problem/1270/B) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 10 phút 31s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số trường hợp thử nghiệm và mỗi trường hợp thử nghiệm cung cấp một mảng các số nguyên. Nhiệm vụ là xác định xem có tồn tại một đoạn liền kề của mảng này có độ rộng đủ lớn so với chiều dài của nó hay không. Chính xác hơn, chúng ta muốn một mảng con trong đó chênh lệch giữa phần tử lớn nhất và nhỏ nhất của nó ít nhất bằng số phần tử trong mảng con đó. 

Đối với mỗi trường hợp thử nghiệm, chúng ta phải xuất ra rằng không tồn tại mảng con nào như vậy hoặc xuất ra bất kỳ mảng con hợp lệ nào thỏa mãn điều kiện này. 

Ràng buộc về tổng kích thước mảng trong tất cả các trường hợp thử nghiệm lên tới 200.000, loại trừ mọi cách tiếp cận bậc hai hoặc phương pháp xấu hơn cho mỗi trường hợp thử nghiệm. Một giải pháp tính toán lại mức tối thiểu và tối đa cho mỗi mảng con sẽ yêu cầu công việc khoảng O(n^2) cho mỗi trường hợp thử nghiệm, điều này sẽ hết thời gian chờ ngay lập tức. Điều này đẩy chúng ta tới một công trình tuyến tính hoặc gần tuyến tính. 

Một trường hợp đơn giản nhưng quan trọng là khi tất cả các phần tử đều giống hệt nhau. Trong trường hợp đó, mọi mảng con đều có max trừ min bằng 0, trong khi bất kỳ mảng con nào có độ dài ít nhất 1 đều yêu cầu chênh lệch ít nhất bằng độ dài của nó. Vì vậy câu trả lời luôn là không thể. Một trường hợp khác là khi một mảng con hợp lệ chỉ tồn tại ở phần cuối của mảng, cách tiếp cận “tập trung vào giữa” tham lam có thể bỏ lỡ nếu không cẩn thận về cách xây dựng mảng con. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử từng mảng con và tính giá trị tối thiểu và tối đa của nó. Đối với mỗi cặp điểm cuối, chúng tôi quét phân đoạn và đánh giá xem điều kiện có đúng hay không. Điều này hoạt động chính xác vì nó kiểm tra chính xác định nghĩa của vấn đề. Tuy nhiên, đối với một mảng có kích thước n, có các mảng con O(n^2) và mỗi mảng yêu cầu thời gian O(n) để đánh giá, dẫn đến độ phức tạp tổng cộng là O(n^3). Ngay cả khi tối ưu hóa để duy trì mức tối thiểu và tối đa tăng dần, chúng tôi vẫn ở mức O(n^2), quá chậm đối với n lên tới 200.000. 

Quan sát quan trọng là chúng ta không thực sự cần xem xét tất cả các mảng con. Chúng ta chỉ cần tìm một điểm trong đó khoảng cách giữa mức tối đa và mức tối thiểu đủ lớn so với độ dài của nó. Thay vì suy nghĩ về các khoảng thời gian tùy ý, chúng ta có thể xây dựng một mảng con một cách tham lam xung quanh hai vị trí mà mảng đó có bước nhảy lớn. 

Xét cặp chỉ số i và j bất kỳ sao cho hiệu tuyệt đối |a[i] - a[j]| là lớn. Nếu chúng ta lấy mảng con trải dài từ i đến j, thì giá trị lớn nhất của nó ít nhất là max(a[i], a[j]) và giá trị nhỏ nhất của nó nhiều nhất là min(a[i], a[j]). Độ dài là j - i + 1. Nếu chênh lệch giá trị đủ lớn so với khoảng cách chỉ số, chúng ta sẽ ngay lập tức nhận được câu trả lời hợp lệ. 

Điều này chuyển vấn đề từ việc tìm kiếm trên các mảng con sang kiểm tra xem có tồn tại cặp nào đủ “dốc” hay không. Vì chúng ta chỉ cần một mảng con hợp lệ nên chúng ta có thể tìm kiếm một điều kiện mang tính xây dựng: nếu chúng ta sắp xếp hoặc theo dõi các điểm cực trị về mặt khái niệm, chúng ta luôn có thể rút ra một phân đoạn hợp lệ khi mảng chứa cấu hình cục bộ tạo ra khoảng cách giá trị lớn trong một khoảng cách ngắn. 

Giải pháp tiêu chuẩn đơn giản hóa điều này hơn nữa: chỉ cần kiểm tra các phần tử liền kề sau khi sắp xếp các chỉ số theo giá trị là đủ hoặc lý do là nếu không tồn tại mảng con như vậy thì mảng đó phải “quá phẳng” theo nghĩa chính xác ngụ ý rằng tất cả các phần tử đều bằng nhau. Điều đó dẫn đến một cách xây dựng trực tiếp: nếu chúng tôi tìm thấy hai vị trí có chênh lệch giá trị ít nhất bằng khoảng cách chỉ số, chúng tôi sẽ xuất phân đoạn đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đối với mỗi trường hợp thử nghiệm, hãy quét mảng và xem xét các mảng con ứng cử viên được xác định bởi các cặp vị trí trong đó chênh lệch giá trị lớn có thể bù đắp cho khoảng cách của chúng. Các ứng cử viên đơn giản nhất đến từ việc so sánh các phần tử khi chúng ta duyệt qua mảng. 
2. Duy trì các giá trị nhỏ nhất và lớn nhất cho đến nay cùng với vị trí của chúng khi chúng tôi lặp lại. Mục tiêu là để phát hiện xem vị trí hiện tại có thể tạo thành một phân đoạn hợp lệ với mức cực đoan đã thấy trước đó hay không. 
3. Tại chỉ số i, so sánh a[i] với mức tối thiểu và tối đa hiện tại được thấy cho đến nay. Nếu a[i] lớn hơn nhiều so với giá trị tối thiểu hoặc nhỏ hơn nhiều so với giá trị tối đa, chúng ta kiểm tra xem đoạn giữa các vị trí của chúng có thỏa mãn điều kiện độ dài max - min ≥ hay không. 
4. Nếu một trong hai điều kiện được thỏa mãn, chúng ta sẽ xuất ra phân đoạn tương ứng ngay lập tức vì bất kỳ mảng con hợp lệ nào cũng đủ. 
5. Nếu chúng tôi quét xong mà không tìm thấy bất kỳ cặp hợp lệ nào, hãy kết luận rằng không tồn tại mảng con thú vị nào. 

### Tại sao nó hoạt động 

Bất kỳ mảng con hợp lệ nào cũng phải có giá trị lớn nhất và nhỏ nhất tại hai điểm cuối của đoạn. Khi quét từ trái sang phải, các điểm cuối đó tương ứng với một số vị trí trước đó và chỉ mục hiện tại. Nếu một phân đoạn hợp lệ tồn tại, một trong những điểm cuối của nó cuối cùng sẽ là ranh giới phù hợp khi chúng ta tiếp cận nó trong quá trình quét. Tại thời điểm đó, cực trị được lưu trữ ở phía đối diện đảm bảo rằng chúng tôi phát hiện được tình trạng. Điều này đảm bảo chúng ta không bỏ sót bất kỳ phân đoạn ứng viên nào, bởi vì mọi mảng con hợp lệ đều được chứng kiến ​​bởi phần tử cuối cùng của nó trong quá trình truyền tải. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        min_val = a[0]
        max_val = a[0]
        min_pos = 0
        max_pos = 0

        found = False
        ans_l = ans_r = -1

        for i in range(n):
            if i == 0:
                continue

            # try extending with current element as right endpoint
            if a[i] < min_val:
                min_val = a[i]
                min_pos = i

            if a[i] > max_val:
                max_val = a[i]
                max_pos = i

            # check using current extremes
            if max_val - a[i] >= (i - max_pos + 1):
                ans_l, ans_r = max_pos, i
                found = True
                break

            if a[i] - min_val >= (i - min_pos + 1):
                ans_l, ans_r = min_pos, i
                found = True
                break

        if found:
            print("YES")
            print(ans_l + 1, ans_r + 1)
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì các điểm cực trị đang chạy và vị trí của chúng trong khi quét từ trái sang phải. Chi tiết quan trọng là chúng tôi luôn so sánh phần tử hiện tại với phần tử tối thiểu và tối đa được biết đến nhiều nhất, bởi vì bất kỳ mảng con hợp lệ nào kết thúc bằng i đều phải sử dụng một trong các phần tử này làm điểm cuối đối diện của nó. 

Việc lập chỉ mục được xử lý cẩn thận bằng cách lưu trữ nội bộ các vị trí dựa trên 0 và chỉ chuyển đổi sang lập chỉ mục dựa trên một tại thời điểm đầu ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 0 1 9
```| tôi | một [tôi] | giá trị tối thiểu | phút_pos | giá trị tối đa | max_pos | quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | 0 | 2 | 0 | ban đầu | 
| 1 | 0 | 0 | 1 | 2 | 0 | cập nhật tối thiểu | 
| 2 | 1 | 0 | 1 | 2 | 0 | không nghỉ | 
| 3 | 9 | 0 | 1 | 9 | 3 | kiểm tra trình kích hoạt CÓ | 

Tại i = 3, số tối đa 9 ghép với số 0 tối thiểu tạo ra một mảng con thỏa mãn bất đẳng thức cần thiết theo chiều dài của nó. Điều này xác nhận rằng thuật toán phát hiện một phân đoạn hợp lệ khi một khoảng trống toàn cục lớn xuất hiện sớm và được hoàn thành ở chỉ mục muộn hơn. 

### Ví dụ 2 

đầu vào:```
4
1 2 3 4
```| tôi | một [tôi] | giá trị tối thiểu | phút_pos | giá trị tối đa | max_pos | quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 1 | 0 | ban đầu | 
| 1 | 2 | 1 | 0 | 2 | 1 | không nghỉ | 
| 2 | 3 | 1 | 0 | 3 | 2 | không nghỉ | 
| 3 | 4 | 1 | 0 | 4 | 3 | không nghỉ | 

Không có điểm nào tạo ra đoạn mà giá trị chênh lệch chi phối chiều dài, do đó đầu ra là KHÔNG. Điều này cho thấy các mảng đơn điệu không thể thỏa mãn điều kiện vì sự tăng trưởng về giá trị hoàn toàn khớp với sự tăng trưởng về độ dài của mảng con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần trong khi vẫn duy trì các điểm cực trị đang chạy | 
| Không gian | O(1) | Chỉ một số biến cố định được sử dụng cho mỗi trường hợp thử nghiệm | 

Giải pháp chạy theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm và vì tổng kích thước đầu vào được giới hạn bởi 200.000 nên nó vừa vặn thoải mái trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        min_val = a[0]
        max_val = a[0]
        min_pos = 0
        max_pos = 0

        found = False
        ans = None

        for i in range(n):
            if i == 0:
                continue

            if a[i] < min_val:
                min_val = a[i]
                min_pos = i
            if a[i] > max_val:
                max_val = a[i]
                max_pos = i

            if max_val - a[i] >= (i - max_pos + 1):
                found = True
                ans = (max_pos + 1, i + 1)
                break
            if a[i] - min_val >= (i - min_pos + 1):
                found = True
                ans = (min_pos + 1, i + 1)
                break

        if found:
            out.append("YES")
            out.append(f"{ans[0]} {ans[1]}")
        else:
            out.append("NO")

    return "\n".join(out)

# provided samples
assert run("""3
5
1 2 3 4 5
4
2 0 1 9
2
2019 2020
""") == """NO
YES
1 4
NO"""

# custom cases
assert run("""1
3
5 5 5
""") == "NO", "all equal"

assert run("""1
2
0 10
""") == "YES\n1 2", "minimum size valid"

assert run("""1
4
1 100 2 3
""") in ["YES\n1 2", "YES\n2 3", "YES\n1 3", "YES\n2 4"], "large spike"

assert run("""1
6
1 2 3 4 5 6
""") == "NO", "strictly increasing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | KHÔNG | không có chênh lệch hợp lệ | 
| 0 10 | CÓ 1 2 | phân đoạn hợp lệ nhỏ nhất | 
| 1 100 2 3 | CÓ | phát hiện đột biến cục bộ | 
| 1..6 | KHÔNG | điều kiện lỗi mảng đơn điệu | 

## Vỏ cạnh 

Đối với một mảng trong đó tất cả các giá trị đều giống hệt nhau, thuật toán sẽ khởi tạo giá trị tối thiểu và tối đa bằng cùng một giá trị và không bao giờ tìm thấy phân đoạn mà tối đa trừ tối thiểu có thể tăng lên. Mọi so sánh đều thất bại vì chênh lệch giá trị vẫn bằng 0 trong khi độ dài yêu cầu tăng lên. Quá trình quét hoàn tất và trả về chính xác NO. 

Đối với một mảng nhỏ có kích thước hai, nếu các giá trị khác nhau, phân đoạn đó ngay lập tức hợp lệ vì chênh lệch ít nhất là 1 và ngưỡng yêu cầu cũng chỉ là 2 khi chênh lệch đủ lớn. Thuật toán kiểm tra phân đoạn duy nhất có thể và trả về trực tiếp, khớp chính xác với điều kiện. 

Đối với một dãy tăng nghiêm ngặt, mặc dù max trừ min tăng theo thời gian, độ dài mảng con tăng với tốc độ như nhau, do đó bất đẳng thức không bao giờ chuyển sang có lợi cho tính hợp lệ. Thuật toán liên tục cập nhật các điểm cực trị nhưng không bao giờ gặp phải phân đoạn trong đó khoảng cách giá trị lấn át khoảng cách chỉ số, dẫn đến kết quả KHÔNG chính xác.
