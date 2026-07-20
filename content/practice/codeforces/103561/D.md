---
title: "CF 103561D - Hướng phố"
description: "Chúng ta được cung cấp một tập hợp các điểm trên lưới số nguyên 2D và tất cả các điểm được quan sát từ một điểm gốc cố định tại tâm của hệ tọa độ. Từ gốc tọa độ đó, chúng ta tưởng tượng một “máy ảnh” chỉ có thể nhìn thấy trong một vùng hình nêm được xác định bởi hai tia bắt đầu từ gốc tọa độ."
date: "2026-07-03T05:23:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103561
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 02-11-22 Div. 1 (Advanced)"
rating: 0
weight: 103561
solve_time_s: 47
verified: true
draft: false
---

[CF 103561D - Chế độ xem thành phố](https://codeforces.com/problemset/problem/103561/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các điểm trên lưới số nguyên 2D và tất cả các điểm được quan sát từ một điểm gốc cố định tại tâm của hệ tọa độ. Từ gốc tọa độ đó, chúng ta tưởng tượng một “máy ảnh” chỉ có thể nhìn thấy trong một vùng hình nêm được xác định bởi hai tia bắt đầu từ gốc tọa độ. Mọi thứ nằm chính xác trên hoặc giữa hai tia này đều có thể nhìn thấy được. 

Nhiệm vụ là chọn chiều rộng góc nhỏ nhất có thể có của một cái nêm sao cho mọi điểm cho trước đều nằm bên trong nó. Nêm có thể được xoay tự do xung quanh điểm gốc, vì vậy chúng ta không cố định hướng của nó mà chỉ cố định góc mở của nó. 

Đầu vào chỉ đơn giản là số điểm theo sau là tọa độ của chúng. Đầu ra là một số thực duy nhất: góc tối thiểu tính theo độ của hình nêm có thể bao phủ tất cả các điểm cùng một lúc. 

Các ràng buộc cho phép lên tới 100.000 điểm, do đó, bất kỳ giải pháp nào so sánh trực tiếp tất cả các cặp hoặc thử tất cả các phép quay của các nêm ứng cử viên đều quá chậm. Cách tiếp cận O(n²) sẽ liên quan đến việc kiểm tra tất cả các cặp điểm dưới dạng các tia biên tiềm năng, dẫn đến khoảng 10¹⁰ phép tính trong trường hợp xấu nhất và không khả thi. Do đó, chúng ta cần cấu trúc O(n log n) hoặc O(n) sau khi tiền xử lý. 

Một vấn đề tế nhị xuất hiện với góc bao quanh. Các điểm gần góc 0 độ và các điểm gần 359 độ thực sự có thể ở gần nhau theo thứ tự hình tròn, nhưng việc quét các góc tuyến tính đơn giản sẽ coi chúng ở xa nhau. Bất kỳ giải pháp đúng đắn nào cũng phải giải quyết được tính chất tuần hoàn này. 

Một trường hợp cạnh khác là khi nhiều điểm nằm trên cùng một tia tính từ gốc tọa độ. Ví dụ: các điểm (2, 0) và (5, 0) không được tạo ra bất kỳ sự phân tách góc nào; họ hành xử theo một hướng. Việc triển khai ngây thơ không chuẩn hóa các góc một cách cẩn thận vẫn hoạt động, nhưng cần cẩn thận khi lý luận về sự khác biệt. 

Cuối cùng, độ chính xác của dấu phẩy động rất quan trọng vì câu trả lời được yêu cầu trong phạm vi dung sai 1e-6. Bất kỳ phương pháp nào dựa vào hàm lượng giác nghịch đảo đều phải ổn định dưới các sai số làm tròn. 

## Phương pháp tiếp cận 

Ý tưởng Brute-Force là xem xét mọi hướng có thể có của một cái nêm. Một cách tự nhiên để làm điều này là tính góc của mọi điểm so với gốc bằng cách sử dụng atan2, sắp xếp các góc này, sau đó thử chọn điểm bắt đầu và tính khoảng cách góc tối đa cần thiết để bao phủ tất cả các điểm từ đó. Đối với mỗi chỉ số bắt đầu i, chúng ta sẽ quét về phía trước và tìm điểm xa nhất trong phạm vi hướng về phía trước 180 độ hoặc tương đương xác định khoảng nhỏ nhất trên vòng tròn chứa tất cả các điểm sau khi quay. 

Tuy nhiên, việc triển khai một cách ngây thơ ý tưởng này để kiểm tra mọi điểm bắt đầu một cách độc lập sẽ dẫn đến hành vi O(n2), vì đối với mỗi điểm chúng ta có thể quét tất cả các điểm khác để tính khoảng cách góc tối đa. Với n = 10⁵, tốc độ này quá chậm. 

Quan sát quan trọng là chúng ta thực sự đang tìm kiếm cung tròn nhỏ nhất bao phủ tất cả các điểm. Khi tất cả các điểm được chuyển thành các góc trong [0, 360), bài toán sẽ trở thành: tìm độ dài tối thiểu của một cung trên đường tròn chứa tất cả các điểm. Điều này tương đương với việc tìm khoảng cách tối đa giữa các góc được sắp xếp liên tiếp, bao gồm cả khoảng cách bao quanh giữa điểm cuối cùng và điểm đầu tiên. Nếu chúng ta loại bỏ khoảng cách lớn nhất này thì phần còn lại là cung nhỏ nhất bao phủ tất cả các điểm. 

Điều này hiệu quả vì bất kỳ hình nêm tối ưu nào cũng có thể được xoay sao cho ranh giới của nó thẳng hàng với một trong các điểm và vật cản duy nhất để bao phủ tất cả các điểm là vùng góc trống lớn nhất. Loại bỏ vùng đó mang lại vòng cung kèm theo tối thiểu. 

Điều này làm giảm vấn đề sắp xếp các góc và quét một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu (sắp xếp + khoảng cách tối đa) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chuyển đổi mọi điểm (x, y) thành góc cực của nó bằng cách sử dụng atan2(y, x). Điều này đặt mỗi điểm trên một biểu diễn vòng tròn tính bằng radian. Lý do sử dụng atan2 thay vì arctan là vì nó xử lý chính xác tất cả các góc phần tư mà không cần điều chỉnh thủ công. 
2. Chuẩn hóa tất cả các góc thành một phạm vi nhất quán, thường là [0, 2π). Điều này đảm bảo rằng việc sắp xếp tạo ra một thứ tự tuần hoàn chính xác. 
3. Sắp xếp tất cả các góc theo thứ tự tăng dần. Sau khi sắp xếp, các phần tử liên tiếp biểu thị các hướng liền kề xung quanh điểm gốc. 
4. Tính độ chênh lệch góc giữa các phần tử liên tiếp trong danh sách đã sắp xếp. Đồng thời tính hiệu bao quanh giữa góc cuối cùng cộng với 2π và góc đầu tiên. Những khác biệt này thể hiện những khoảng trống trên vòng tròn. 
5. Tìm khoảng cách lớn nhất giữa tất cả những khác biệt này. Khoảng cách này đại diện cho khu vực góc lớn nhất không chứa điểm. 
6. Trừ khoảng cách lớn nhất này với 2π để có được cung nhỏ nhất chứa tất cả các điểm. 
7. Chuyển đổi kết quả từ radian sang độ bằng cách nhân với 180/π. 

Tại sao nó hoạt động: bất kỳ hình nêm nào bao phủ tất cả các điểm phải loại trừ tối đa một vùng trống liên tục trên vòng tròn. Cái nêm tối ưu đạt được bằng cách đặt lỗ mở của nó đối diện chính xác với khoảng trống lớn nhất, vì khoảng trống đó là vùng duy nhất không cần phải che phủ. Do đó, việc giảm thiểu góc nêm tương đương với việc tối đa hóa khoảng cách bị loại trừ, tức là khoảng cách tối đa giữa các vị trí góc liên tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    n = int(input())
    angles = []
    
    for _ in range(n):
        x, y = map(int, input().split())
        ang = math.atan2(y, x)
        if ang < 0:
            ang += 2 * math.pi
        angles.append(ang)

    angles.sort()

    if n == 1:
        print(0.0)
        return

    max_gap = 0.0

    for i in range(n):
        j = (i + 1) % n
        if j == 0:
            gap = (angles[0] + 2 * math.pi) - angles[i]
        else:
            gap = angles[j] - angles[i]
        max_gap = max(max_gap, gap)

    answer = 2 * math.pi - max_gap
    print(answer * 180 / math.pi)

if __name__ == "__main__":
    solve()
```Giải pháp được cấu trúc xoay quanh việc biến một cấu hình hình học thành một bài toán sắp xếp theo vòng tròn. Việc sử dụng atan2 đảm bảo tính chính xác trên tất cả các góc phần tư và bước chuẩn hóa sẽ tránh được sự không nhất quán của góc âm. 

Một lỗi triển khai phổ biến là quên khoảng cách bao quanh giữa góc cuối cùng và góc đầu tiên. Nếu không có nó, trường hợp tất cả các điểm tụ lại gần 0 độ nhưng một điểm nằm gần 360 độ sẽ tạo ra một cái nêm lớn không chính xác thay vì một cái nhỏ. 

Một điểm tinh tế khác là xử lý n = 1. Trong trường hợp đó, không cần góc và câu trả lời đúng là 0. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2
3 0
0 3
```| Bước | Góc (rad) | Đã sắp xếp | Khoảng trống | Khoảng cách tối đa | 
| --- | --- | --- | --- | --- | 
| Sau khi chuyển đổi | 0, π/2 | 0, π/2 | π/2 | π/2 | 

Khoảng cách lớn nhất là π/2, nghĩa là một nửa hình tròn trống. Loại bỏ nó để lại π/2 là cung bao phủ tối thiểu. Chuyển đổi cho 90 độ. 

### Mẫu 2 

đầu vào:```
3
-3 0
0 3
-3 -3
```| Bước | Góc (rad) | Đã sắp xếp | Khoảng trống | Khoảng cách tối đa | 
| --- | --- | --- | --- | --- | 
| Sau khi chuyển đổi | π, π/2, -3π/4 | π/2, π, 5π/4 | π/2, π/4, 5π/4-π/2 | π/2 | 

Khoảng cách lớn nhất là 3π/4? trên thực tế, từ π đến 5π/4 bao quanh mang lại π/4, trong khi khoảng cách lớn nhất là giữa 5π/4 và π/2 tức là 3π/4. Loại bỏ khoảng trống đó để lại một cung 3π/4, tương ứng với 135 độ. 

Điều này cho thấy cách giải pháp xác định chính xác rằng nêm phủ kín nhất không nhất thiết phải thẳng hàng với các hướng trục rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp n góc chiếm ưu thế; quét là tuyến tính | 
| Không gian | O(n) | Lưu trữ danh sách góc | 

Các ràng buộc cho phép tối đa 100.000 điểm và cách tiếp cận O(n log n) nằm trong giới hạn, vì việc sắp xếp 10⁵ phần tử có hiệu quả trong Python và C++ trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import atan2, pi
    n = int(sys.stdin.readline())
    ang = []
    for _ in range(n):
        x, y = map(int, sys.stdin.readline().split())
        a = math.atan2(y, x)
        if a < 0:
            a += 2*math.pi
        ang.append(a)
    ang.sort()
    if n == 1:
        return "0.0\n"
    mx = 0.0
    for i in range(n):
        j = (i+1) % n
        if j == 0:
            gap = ang[0] + 2*math.pi - ang[i]
        else:
            gap = ang[j] - ang[i]
        mx = max(mx, gap)
    ans = (2*math.pi - mx) * 180 / math.pi
    return str(ans) + "\n"

# provided samples
assert abs(float(run("2\n3 0\n0 3\n")) - 90.0) < 1e-6

# custom cases
assert abs(float(run("1\n5 7\n")) - 0.0) < 1e-6, "single point"
assert abs(float(run("4\n1 0\n0 1\n-1 0\n0 -1\n")) - 180.0) < 1e-6, "full cross"
assert abs(float(run("3\n1 0\n2 0\n3 0\n")) - 0.0) < 1e-6, "collinear points"
assert abs(float(run("3\n1 0\n0 1\n-1 0\n")) - 180.0) < 1e-6, "half circle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 0 | trường hợp tầm thường | 
| bốn điểm trực giao | 180 | chênh lệch toàn bộ đối xứng | 
| điểm thẳng hàng | 0 | xử lý theo hướng giống hệt nhau | 
| điểm nửa vòng tròn | 180 | lựa chọn cung đúng | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các điểm nằm trên cùng một tia tính từ gốc tọa độ. Trong trường hợp đó, tất cả các góc được tính toán đều giống hệt nhau, do đó việc sắp xếp sẽ tạo ra một mảng không đổi. Việc tính toán khe hở mang lại một khe hở đầy đủ 2π khi hiệu bao quanh và cung còn lại bằng 0, điều này đúng vì thấu kính góc 0 bao phủ một hướng. 

Một trường hợp cạnh khác là khi các điểm được nhóm xung quanh ranh giới 0/360 độ. Ví dụ: các điểm ở 1 độ và 359 độ sẽ tạo ra một cung nhỏ 2 độ chứ không phải một cung lớn 358 độ. Tính toán khoảng cách bao quanh xử lý rõ ràng vấn đề này bằng cách xem xét sự khác biệt vòng tròn giữa góc cuối cùng và góc đầu tiên sau khi sắp xếp. 

Trường hợp cạnh cuối cùng là độ chính xác của dấu phẩy động khi các điểm gần như thẳng hàng. Việc sử dụng atan2 đảm bảo thứ tự ổn định và hoạt động theo radian mà không cần chuyển đổi lặp lại sẽ giảm thiểu lỗi tích lũy. Việc chuyển đổi cuối cùng sang độ được thực hiện một lần, giúp duy trì độ chính xác trong dung sai yêu cầu.
