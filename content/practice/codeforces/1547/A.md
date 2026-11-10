---
title: "CF 1547A - Con đường ngắn nhất có chướng ngại vật"
description: "Chúng tôi đang làm việc trên một mạng lưới vô hạn trong đó chuyển động được phép theo bốn hướng: lên, xuống, trái và phải, mỗi hướng tốn một bước. Chúng ta có ba ô đặc biệt: ô bắt đầu, ô đích và ô cấm không thể bước lên."
date: "2026-06-14T19:49:34+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1547
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 731 (Div. 3)"
rating: 800
weight: 1547
solve_time_s: 460
verified: true
draft: false
---

[CF 1547A - Con đường ngắn nhất có chướng ngại vật](https://codeforces.com/problemset/problem/1547/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 7 phút 40 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một mạng lưới vô hạn trong đó chuyển động được phép theo bốn hướng: lên, xuống, trái và phải, mỗi hướng tốn một bước. Chúng ta có ba ô đặc biệt: ô bắt đầu, ô đích và ô cấm không thể bước lên. Nhiệm vụ là tính toán số lần di chuyển ngắn nhất cần thiết để đi từ đầu đến mục tiêu mà không bao giờ hạ cánh xuống ô cấm. 

Nếu không có ô cấm, câu trả lời sẽ đơn giản là khoảng cách Manhattan giữa điểm bắt đầu và mục tiêu, vì trong một lưới có chuyển động tự do, đường đi ngắn nhất luôn là tổng của các khoảng cách ngang và dọc. 

Giới hạn tối đa 10^4 trường hợp thử nghiệm buộc chúng tôi phải tránh mọi tìm kiếm biểu đồ cho mỗi truy vấn. BFS trên lưới sẽ quá chậm vì mặc dù tọa độ được giới hạn bởi 1000, nhưng về mặt khái niệm, lưới không bị giới hạn và độ phức tạp của BFS phụ thuộc vào khu vực được khám phá chứ không phải kích thước đầu vào. Chúng ta cần một công thức O(1) cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh khóa phát sinh khi ô bị cấm nằm chính xác trên đường thẳng Manhattan giữa A và B. Ví dụ: nếu A là (1,1), B là (1,5) và F là (1,3), thì đường thẳng đứng trực tiếp bị chặn. Một phép tính khoảng cách đơn giản ở Manhattan sẽ trả về 4, nhưng câu trả lời đúng là 6 vì chúng ta phải đi vòng quanh F. Một trường hợp tinh tế khác là khi F không nằm trên bất kỳ hành lang đường đi ngắn nhất nào giữa A và B; thì câu trả lời vẫn không thay đổi. 

## Phương pháp tiếp cận 

Không có trở ngại, vấn đề là chuyện nhỏ. Đường đi ngắn nhất giữa hai điểm lưới chỉ đơn giản là khoảng cách Manhattan, được tính bằng chênh lệch tuyệt đối ở tọa độ x cộng với chênh lệch tuyệt đối ở tọa độ y. Điều này hiệu quả vì mọi đường đi tối ưu phải thực hiện chính xác nhiều bước di chuyển theo chiều ngang và chiều dọc và mọi hoán vị của những bước di chuyển đó đều hợp lệ. 

Việc giới thiệu một ô cấm duy nhất chỉ thay đổi mọi thứ theo một cách rất hạn chế. Điểm duy nhất mà câu trả lời khác với khoảng cách Manhattan là khi mọi đường đi ngắn nhất từ ​​A đến B buộc phải đi qua F. Điều này xảy ra chính xác khi A, B và F nằm trên cùng một hàng hoặc đoạn cột và F nằm giữa chúng. Trong tình huống đó, bất kỳ con đường ngắn nhất nào cũng sẽ bước lên F, vì vậy chúng ta phải đi vòng bằng cách đi vòng qua nó, điều này sẽ bổ sung thêm chính xác hai bước. 

Nếu F không nằm trên đoạn hình học được tạo bởi A và B theo nghĩa Manhattan, chúng ta vẫn có thể đạt được khoảng cách Manhattan bằng cách điều chỉnh một chút thứ tự di chuyển để chúng ta không bao giờ đáp xuống F. Điều này có thể thực hiện được vì luôn có ít nhất một tuyến đường ngắn nhất thay thế tránh được một điểm chệch hướng. 

Vì vậy, giải pháp giảm xuống việc tính toán khoảng cách Manhattan và sau đó kiểm tra xem F có chặn sự thẳng hàng trực tiếp của A và B trên một đoạn thẳng hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS vũ phu trên lưới | O(N) cho mỗi truy vấn (không bị giới hạn) | O(N) | Quá chậm | 
| Manhattan + kiểm tra chặn | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính khoảng cách Manhattan giữa A và B làm đáp án cơ bản. Điều này thể hiện con đường ngắn nhất có thể bỏ qua chướng ngại vật. 
2. Kiểm tra xem A và B có được căn chỉnh theo chiều ngang hoặc chiều dọc hay không. Nếu chúng không được căn chỉnh theo cách này thì không một điểm nào có thể buộc tất cả các đường đi ngắn nhất đi qua F, do đó câu trả lời cơ bản vẫn hợp lệ. 
3. Nếu A và B có cùng tọa độ x thì mọi đường đi ngắn nhất chỉ di chuyển theo chiều dọc. Trong trường hợp này, hãy kiểm tra xem F có nằm trên cùng một đường thẳng đứng này không và tọa độ y của nó có nằm hoàn toàn giữa A và B hay không. 
4. Nếu A và B có cùng tọa độ y, hãy thực hiện kiểm tra đối xứng để căn chỉnh theo chiều ngang. 
5. Nếu F nằm chính xác trên đoạn giữa A và B trong cả hai trường hợp, hãy tăng câu trả lời lên 2 để tính đến đường vòng quanh ô bị chặn.

Chi phí đi đường vòng đúng bằng 2 xuất phát từ thực tế là cách nhỏ nhất để tránh ô trung gian bị chặn trên một đường thẳng là bước sang một đơn vị, đi vòng qua nó và quay trở lại đường ban đầu. 

### Tại sao nó hoạt động 

Đường đi ngắn nhất trong lưới được xác định đầy đủ bằng cách chọn thứ tự di chuyển ngang và dọc. Một ô bị chặn chỉ có thể vô hiệu hóa tất cả các đường dẫn ngắn nhất nếu nó nằm trên mọi đường dẫn đơn điệu duy trì độ dài tối ưu. Điều đó chỉ xảy ra khi đường dẫn bị giới hạn ở một đoạn hàng hoặc cột. Bên ngoài cấu hình này, chúng ta luôn có thể sắp xếp lại các bước di chuyển để bỏ qua ô bị cấm mà không làm tăng tổng chiều dài đường đi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    xA, yA = map(int, input().split())
    xB, yB = map(int, input().split())
    xF, yF = map(int, input().split())

    ans = abs(xA - xB) + abs(yA - yB)

    if xA == xB == xF:
        if min(yA, yB) < yF < max(yA, yB):
            ans += 2

    if yA == yB == yF:
        if min(xA, xB) < xF < max(xA, xB):
            ans += 2

    return ans

t = int(input())
out = []
for _ in range(t):
    input()
    out.append(str(solve()))

print("\n".join(out))
```Việc triển khai tính toán trực tiếp khoảng cách Manhattan và sau đó áp dụng hiệu chỉnh theo thời gian không đổi nếu ô bị cấm nằm hoàn toàn giữa A và B trên một trục chung. Dòng trống giữa các trường hợp thử nghiệm được sử dụng rõ ràng vì định dạng đầu vào bao gồm nó. 

Một lỗi triển khai phổ biến là quên kiểm tra bất đẳng thức nghiêm ngặt. Nếu F bằng A hoặc B, nó sẽ không kích hoạt bất kỳ logic đường vòng nào vì vấn đề đảm bảo các ô riêng biệt, nhưng ngay cả khi giả định đó được nới lỏng, các điểm cuối không chặn các đường đi ngắn nhất theo cách tương tự. 

## Ví dụ đã hoạt động 

Xét trường hợp A = (1,1), B = (3,3) và F = (2,2). Khoảng cách Manhattan là 4. Các ô không được căn chỉnh theo chiều ngang hoặc chiều dọc nên không áp dụng hiệu chỉnh. 

| Bước | điều kiện x | điều kiện y | sửa chữa | trả lời | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | - | 0 | 4 | 

Điều này cho thấy các trường hợp di chuyển theo đường chéo không bị ảnh hưởng bởi một chướng ngại vật duy nhất không nằm trên hành lang cưỡng bức. 

Bây giờ xét A = (2,5), B = (2,1), F = (2,3). Khoảng cách Manhattan là 4. Vì tất cả các điểm đều có chung x = 2 và F nằm giữa y = 5 và y = 1 nên mọi đường đi ngắn nhất theo chiều dọc trực tiếp đều đi qua F. 

| Bước | căn chỉnh | F giữa | sửa chữa | trả lời | 
| --- | --- | --- | --- | --- | 
| ban đầu | dọc | vâng | +2 | 6 | 

Điều này xác nhận quy tắc đi đường vòng, trong đó đường đi phải di chuyển sang một bên một bước rồi quay trở lại, tăng độ dài lên đúng 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi bằng cách sử dụng số học và so sánh | 
| Không gian | O(1) | Chỉ một số biến cố định được lưu trữ | 

Các ràng buộc cho phép tối đa 10^4 truy vấn, do đó cần có giải pháp O(1) cho mỗi truy vấn. Cách tiếp cận đáp ứng điều này một cách dễ dàng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve_case():
        xA, yA = map(int, input().split())
        xB, yB = map(int, input().split())
        xF, yF = map(int, input().split())

        ans = abs(xA - xB) + abs(yA - yB)

        if xA == xB == xF and min(yA, yB) < yF < max(yA, yB):
            ans += 2
        if yA == yB == yF and min(xA, xB) < xF < max(xA, xB):
            ans += 2

        return ans

    t_line = input().strip()
    if not t_line:
        t_line = input().strip()
    t = int(t_line)

    res = []
    for _ in range(t):
        input()  # empty line
        res.append(str(solve_case()))
    return "\n".join(res)

# provided samples (abbreviated formatting assumed consistent)
sample_in = """7

1 1
3 3
2 2

2 5
2 1
2 3

1000 42
1000 1
1000 1000

1 10
3 10
2 10

3 8
7 8
3 7

2 1
4 1
1 1

1 344
1 10
1 1
"""

assert run(sample_in).split() == ["4","6","41","4","4","2","334"]

# custom cases
assert run("""1

1 1
1 5
1 3
""") == "6"

assert run("""1

1 1
5 5
10 10
""") == "8"

assert run("""1

2 2
2 2
1 1
""") == "0"

assert run("""1

3 1
1 1
2 1
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khối dọc | 6 | chướng ngại vật trên đoạn thẳng đứng | 
| đường chéo không có tác dụng | 8 | chướng ngại vật không liên quan trên đường đi | 
| cùng điểm | 0 | khoảng cách suy biến bằng 0 | 
| khối ngang | 4 | vật cản ngang đối xứng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi A và B thẳng hàng nhưng F nằm ngoài đoạn thẳng. Ví dụ A = (1,1), B = (1,5), F = (1,10). Thuật toán kiểm tra bất đẳng thức nghiêm ngặt, do đó không áp dụng hiệu chỉnh và đáp án vẫn là 4. Điều này đúng vì chúng ta có thể định tuyến đường đi lên mà không cần chạm vào F. 

Một trường hợp khác là khi căn chỉnh giữ nguyên x hoặc y nhưng F trùng với một điểm cuối. Vấn đề đảm bảo các điểm khác biệt, nhưng ngay cả khi không, các điểm cuối cũng không bắt buộc phải đi đường vòng vì đường đi bắt đầu hoặc kết thúc ở đó. 

Trường hợp cuối cùng là khi A và B không thẳng hàng, ngay cả khi F chia sẻ tọa độ với một trong số chúng. Đường đi Manhattan luôn có thể được sắp xếp lại để tránh F trong khi vẫn giữ nguyên độ dài, do đó không cần hiệu chỉnh.
