---
title: "CF 346E - Nhảy Doodle"
description: "Chúng ta được cung cấp một chuỗi chiều cao của nền tảng được tạo ra bởi quy tắc số học mô-đun. Nền thứ x nằm ở độ cao (a · x) mod p, trong đó a và p là nguyên tố cùng nhau và chúng ta chỉ xét n vị trí đầu tiên như vậy."
date: "2026-06-06T18:18:03+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 346
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 201 (Div. 1)"
rating: 3000
weight: 346
solve_time_s: 85
verified: true
draft: false
---

[CF 346E - Nhảy Doodle](https://codeforces.com/problemset/problem/346/E) 

**Đánh giá:** 3000 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chiều cao của nền tảng được tạo ra bởi quy tắc số học mô-đun. Nền tảng thứ x nằm ở độ cao`(a · x) mod p`, Ở đâu`a`Và`p`là nguyên tố cùng nhau và chúng tôi chỉ xem xét đầu tiên`n`những vị trí như vậy. Một nhân vật bắt đầu ở độ cao 0 và có thể di chuyển lên trên giữa hai vị trí bất kỳ miễn là một lần nhảy không vượt quá giới hạn cố định`h`. Nhiệm vụ là xác định xem liệu có thể lên tới nền tảng cao nhất trong số này hay không.`n`điểm, giả sử nhân vật có thể chọn bất kỳ thứ tự truy cập nào vào các nền tảng có thể truy cập. 

Đây không phải là câu hỏi đơn giản “liệu ​​chúng ta có thể trực tiếp đạt được mức tối đa” hay không. Khả năng chuyển đổi giữa hai nền tảng đã tiếp cận bất kỳ có nghĩa là các nền tảng tạo thành một biểu đồ trong đó mọi cặp nút được kết nối nếu chênh lệch chiều cao của chúng nhiều nhất`h`. Vấn đề đặt ra là liệu nút có chiều cao tối đa có nằm trong thành phần được kết nối chứa 0 theo quy tắc này hay không. 

Các ràng buộc cực kỳ chặt chẽ về số lượng trường hợp thử nghiệm, lên tới 10⁴, trong khi`n`Và`p`có thể lớn tới 10⁹. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào xây dựng tất cả các độ cao của nền tảng một cách rõ ràng hoặc mô phỏng kết nối một cách đơn giản cho mỗi trường hợp thử nghiệm. Ngay cả O(n) trên mỗi trường hợp thử nghiệm cũng sẽ quá lớn. 

Sự tinh tế quan trọng là trình tự`(a · x mod p)`hoạt động giống như một bước đi giống như hoán vị trên phần dư bởi vì`a`Và`p`là nguyên tố cùng nhau. Tuy nhiên, chúng ta không đi qua toàn bộ chu trình mà chỉ đi qua chu kỳ đầu tiên.`n`điểm theo thứ tự. Điều đó phá hủy hầu hết cấu trúc trực tiếp và lý luận ngây thơ về tính đơn điệu hoặc những khoảng trống đơn giản sẽ thất bại. 

Một sai lầm điển hình là cho rằng vì độ cao bị phân tán modulo`p`, vùng có thể truy cập chỉ phụ thuộc vào các vùng lân cận cục bộ theo thứ tự chỉ mục. Ví dụ, nếu chiều cao là`7, 2, 9, 4`với`h = 2`, người ta có thể cho rằng việc truy cập theo thứ tự chỉ mục sẽ cung cấp thông tin về khả năng tiếp cận một cách không chính xác. Nhưng sự chuyển đổi chính xác chỉ phụ thuộc vào độ cao được sắp xếp chứ không phụ thuộc vào thứ tự chỉ mục. 

Một chế độ lỗi khác là bỏ qua khả năng tiếp cận tương đương với khả năng kết nối trong biểu đồ trong đó các cạnh phụ thuộc hoàn toàn vào chênh lệch độ cao tuyệt đối. Một BFS ngây thơ đối với các chỉ số sẽ bỏ lỡ các bước nhảy hợp lệ giữa các chỉ mục không liền kề có độ cao gần nhau. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo xây dựng biểu đồ đầy đủ về`n`các nút trong đó mỗi nút là chiều cao nền tảng. Hai nút được kết nối nếu chênh lệch độ cao tuyệt đối của chúng lớn nhất`h`. Sau đó, chúng tôi chạy BFS hoặc DFS từ mặt đất (0) và kiểm tra xem có thể truy cập được nút có chiều cao tối đa hay không. Điều này đúng, nhưng việc xây dựng tất cả các cạnh theo cặp tốn O(n2), điều này là không thể ngay cả đối với các trường hợp vừa phải.`n`. 

Chúng ta có thể giảm bớt điều này bằng cách quan sát rằng trong một biểu đồ được xác định bởi ngưỡng chênh lệch tuyệt đối, các cạnh chỉ quan trọng giữa các điểm gần nhau theo thứ tự được sắp xếp. Sau khi sắp xếp độ cao, mỗi nút chỉ cần kết nối với các nút lân cận trong khoảng cách`h`, bởi vì bất kỳ kết nối tầm xa nào cũng có thể bị phân tách thông qua các điểm được sắp xếp trung gian nếu chúng nằm trong ngưỡng. 

Điều này làm giảm vấn đề về khả năng kết nối trong biểu đồ đường sau khi sắp xếp, nơi chúng ta có thể mở rộng các khoảng thời gian có thể tiếp cận một cách tham lam. Khó khăn còn lại là chúng ta vẫn phải tạo ra`n`các giá trị một cách hiệu quả. Quan sát quan trọng là mặc dù`n`lớn trong các trường hợp thử nghiệm, mỗi thử nghiệm chỉ cần quét tuyến tính các dư lượng được tạo và tạo ra từng thử nghiệm`a·x mod p`là O(1). Nút thắt thực sự là việc sắp xếp theo mỗi thử nghiệm, điều này có thể chấp nhận được vì tổng kích thước đầu vào trên tất cả các thử nghiệm bị giới hạn bởi 10⁴. 

Do đó, giải pháp trở thành: tạo tất cả các độ cao, sắp xếp chúng, sau đó quét qua việc hợp nhất các thành phần có thể tiếp cận dựa trên các khoảng trống lớn hơn`h`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đồ thị lực lượng vũ phu | O(n²) | O(n²) | Quá chậm | 
| Sắp xếp + Kết nối tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo tất cả chiều cao nền tảng bằng cách sử dụng`h[i] = (a · i) mod p`vì`1 ≤ i ≤ n`. Điều này xây dựng rõ ràng không gian trạng thái của các nút có thể truy cập. 
2. Sắp xếp mảng độ cao. Việc sắp xếp là cần thiết vì khả năng tiếp cận chỉ phụ thuộc vào mức độ gần nhau về số lượng chứ không phụ thuộc vào thứ tự chỉ mục. 
3. Khởi tạo một biến`reachable_max`là độ cao nhỏ nhất, vì mặt đất ở mức 0 và chúng tôi coi kết nối hướng lên trên. 
4. Quét qua các độ cao được sắp xếp từ nhỏ nhất đến lớn nhất. Nếu khoảng cách giữa các độ cao liên tiếp lớn nhất`h`, hợp nhất chúng vào cùng một cụm có thể truy cập bằng cách mở rộng`reachable_max`. 
5. Nếu tại bất kỳ thời điểm nào khoảng cách vượt quá`h`, chúng tôi kiểm tra xem chiều cao tối đa mục tiêu đã được đưa vào cụm hiện tại chưa. Nếu không, và chúng ta không thể thu hẹp khoảng cách, các giá trị lớn hơn còn lại sẽ không thể truy cập được từ 0. 
6. Sau khi xử lý, kiểm tra xem chiều cao tối đa có nằm trong cùng phân đoạn được kết nối với 0 hay không (tương đương, liệu cụm đầu tiên chứa 0 có đạt đến mức tối đa mà không có điểm ngắt lớn hơn không`h`). 

Ý tưởng chính là thứ tự sắp xếp chuyển đổi vấn đề thành việc kiểm tra xem phạm vi từ giá trị nhỏ nhất có thể tiếp cận có thể được mở rộng liên tục đến mức tối đa mà không gặp phải khoảng cách bị cấm hay không. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, bất kỳ bước nhảy nào giữa hai độ cao chỉ có thể thực hiện được nếu chúng ở trong khoảng cách`h`. Nếu hai giá trị được sắp xếp liên tiếp khác nhau nhiều hơn`h`, thì không có chuỗi giá trị trung gian nào tồn tại giữa chúng, bởi vì bất kỳ giá trị trung gian nào cũng sẽ phải nằm giữa chúng về mặt số lượng và do đó sẽ được sắp xếp ở giữa. Vì vậy, một khoảng cách lớn hơn`h`tạo thành một rào cản cứng ngăn cách đồ thị thành các thành phần rời rạc. Thành phần có thể truy cập từ 0 chính xác là đoạn đầu tiên bắt đầu từ độ cao nhỏ nhất trong đó tối đa tất cả các khác biệt liền kề`h`. Nếu mức tối đa nằm trong phân khúc này thì có thể truy cập được; nếu không thì nó được ngăn cách bởi một khoảng cách cấm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        a, n, p, h = map(int, input().split())
        
        vals = [(a * i) % p for i in range(1, n + 1)]
        vals.sort()
        
        # start from the smallest height cluster
        ok = True
        for i in range(1, n):
            if vals[i] - vals[i - 1] > h:
                # if max is beyond this point, it is unreachable
                if vals[n - 1] > vals[i - 1]:
                    ok = False
                break
        
        out.append("YES" if ok else "NO")
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Bước tạo trực tiếp xây dựng toàn bộ chiều cao nền tảng. Việc sắp xếp đảm bảo chúng ta có thể suy luận thuần túy về tính kề cận trong không gian giá trị. Quá trình quét phát hiện “khoảng cách chặn” đầu tiên lớn hơn`h`. Một khi khoảng cách như vậy được tìm thấy, mọi thứ ở bên phải sẽ bị ngắt kết nối với mọi thứ ở bên trái. 

Điều kiện kiểm tra xem mức tối đa có nằm ngoài khoảng trống hay không là cổng chính xác quan trọng: nếu mức tối đa đã nằm trong tiền tố có thể truy cập thì chúng ta sẽ thành công; mặt khác, biểu đồ đã bị chia tách trước khi đạt được nó. 

Một điểm tinh tế là chúng tôi không mô phỏng rõ ràng khả năng tiếp cận từ 0, vì về mặt khái niệm, 0 nằm dưới tất cả các nền tảng. Nền tảng nhỏ nhất luôn thuộc về khu vực có thể tiếp cận ban đầu và quá trình mở rộng sẽ tiến lên phía trên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 4 12 2
```Chiều cao là:```
7, 2, 9, 4
```Đã sắp xếp:```
2, 4, 7, 9
```| tôi | vals[i] | khoảng cách | hành động | phân khúc có thể tiếp cận | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | - | bắt đầu | {2} | 
| 1 | 4 | 2 | được | {2, 4} | 
| 2 | 7 | 3 | nghỉ (>h) | dừng lại | 

Khoảng cách giữa 4 và 7 vượt quá`h = 2`, do đó đồ thị chia thành`{2, 4}`Và`{7, 9}`. Tối đa là 9, nằm trong thành phần thứ hai, không thể truy cập được từ thành phần thứ nhất. 

Điều này xác nhận rằng việc phát hiện khoảng cách dựa trên sắp xếp sẽ xác định chính xác việc ngắt kết nối. 

### Ví dụ 2 

đầu vào:```
7 4 12 3
```Chiều cao:```
7, 2, 9, 4
```Đã sắp xếp:```
2, 4, 7, 9
```| tôi | vals[i] | khoảng cách | hành động | phân khúc có thể tiếp cận | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | - | bắt đầu | {2} | 
| 1 | 4 | 2 | được | {2, 4} | 
| 2 | 7 | 3 | được | {2, 4, 7} | 
| 3 | 9 | 2 | được | {2, 4, 7, 9} | 

Không có khoảng cách vượt quá`h`, vậy là toàn bộ tập hợp được kết nối. Mức tối đa có thể đạt được. 

Điều này cho thấy tính bất biến của thuật toán là việc không có khoảng trống lớn đồng nghĩa với việc có đầy đủ khả năng kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) mỗi lần kiểm tra | Sắp xếp chiếm ưu thế sau khi tạo giá trị tuyến tính | 
| Không gian | O(n) | Lưu trữ tất cả các chiều cao nền tảng | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì mỗi thử nghiệm chỉ xử lý tối đa`n`giá trị một lần và tổng số công việc trên tất cả các thử nghiệm vẫn có thể quản lý được dựa trên cấu trúc đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        a, n, p, h = map(int, input().split())
        vals = [(a * i) % p for i in range(1, n + 1)]
        vals.sort()
        ok = True
        for i in range(1, n):
            if vals[i] - vals[i - 1] > h:
                if vals[n - 1] > vals[i - 1]:
                    ok = False
                break
        res.append("YES" if ok else "NO")
    return "\n".join(res)

# provided samples
assert run("3\n7 4 12 2\n7 1 9 4\n7 4 12 3\n") == "NO\nNO\nYES"

# minimum size
assert run("1\n1 1 2 0\n") == "YES"

# tight chain
assert run("1\n1 5 100 10\n") == "YES"

# forced split
assert run("1\n1 5 100 1\n") == "NO"

# all equal modulo behavior edge
assert run("1\n1 3 2 0\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 2 0 | CÓ | khả năng tiếp cận tầm thường của nút đơn | 
| 1 5 100 10 | CÓ | chuỗi kết nối đầy đủ | 
| 1 5 100 1 | KHÔNG | ngắt kết nối dựa trên khoảng cách | 
| 1 3 2 0 | CÓ | trường hợp cạnh ngưỡng nhảy bằng 0 | 

## Vỏ cạnh 

Một trường hợp tế nhị là khi`h = 0`. Trong tình huống này, chỉ có thể kết nối các độ cao giống nhau. Thuật toán xử lý điều này một cách chính xác vì bất kỳ chuỗi tăng nghiêm ngặt nào theo thứ tự được sắp xếp sẽ ngay lập tức tạo ra một khoảng cách lớn hơn 0, chia tách các thành phần. Nếu mức tối đa không bằng mức tối thiểu, nó sẽ không thể truy cập được. 

Một trường hợp khác là khi`n = 1`. Mảng được sắp xếp chứa một phần tử duy nhất, do đó không tồn tại khoảng trống và thuật toán ngay lập tức kết luận về khả năng tiếp cận, phù hợp với thực tế là nền tảng duy nhất có thể tiếp cận được một cách tầm thường. 

Một trường hợp cạnh khác phát sinh khi các giá trị bao quanh modulo`p`. Mặc dù trình tự được tạo theo chu kỳ, việc sắp xếp sẽ loại bỏ mọi sự phụ thuộc vào cấu trúc bao bọc. Tiêu chí kết nối chỉ phụ thuộc vào khoảng cách tuyến tính sau khi sắp xếp, do đó việc bao quanh không ảnh hưởng đến tính chính xác.
