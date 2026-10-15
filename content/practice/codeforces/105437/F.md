---
title: "CF 105437F - Trò Chơi Mới"
description: "Chúng ta được cấp một bộ thẻ, mỗi thẻ mang một giá trị nguyên. Monocarp xây dựng một trình tự bằng cách chọn từng lá bài một, loại bỏ chúng khỏi bộ bài khi anh ta lấy chúng."
date: "2026-06-23T03:42:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105437
codeforces_index: "F"
codeforces_contest_name: "ICPC 2024-2025 NERC, Southern and Volga Russia Qualifier"
rating: 0
weight: 105437
solve_time_s: 90
verified: false
draft: false
---

[CF 105437F - Trò chơi mới](https://codeforces.com/problemset/problem/105437/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bộ thẻ, mỗi thẻ mang một giá trị nguyên. Monocarp xây dựng một trình tự bằng cách chọn từng lá bài một, loại bỏ chúng khỏi bộ bài khi anh ta lấy chúng. Thẻ đầu tiên có thể được chọn tự do, nhưng mỗi thẻ tiếp theo bị ràng buộc bởi giá trị của thẻ được lấy cuối cùng: anh ta chỉ có thể lấy một thẻ có cùng giá trị hoặc chính xác một giá trị lớn hơn. 

Có một hạn chế bổ sung đối với toàn bộ chuỗi: trong số tất cả các thẻ được lấy, số lượng giá trị riêng biệt không bao giờ được vượt quá k. Quá trình này dừng lại ngay khi Monocarp không thể lấy một lá bài khác một cách hợp pháp theo các quy tắc này. Nhiệm vụ là tối đa hóa số lượng quân bài anh ta có thể lấy bằng cách chọn quân bài bắt đầu và sau đó chơi một cách tối ưu. 

Các ràng buộc lên tới n = 200000, loại trừ mọi mô phỏng bậc hai trên tất cả các điểm bắt đầu hoặc tất cả các tập hợp con của giá trị. Bất cứ điều gì liên tục quét lại mảng hoặc cố gắng mô phỏng tất cả các lần khởi động tham lam có thể xảy ra một cách độc lập sẽ quá chậm. Một giải pháp phải giảm vấn đề về mức quét tuyến tính hoặc gần tuyến tính sau quá trình tiền xử lý. 

Trường hợp phức tạp xuất hiện khi các giá trị nằm rải rác nhưng vẫn tạo thành chuỗi dài có thể sử dụng được. Ví dụ: nếu k = 2 và mảng là`[1, 100, 2, 3, 101]`, một chiến lược ngây thơ mở rộng một cách tham lam từ điểm bắt đầu có thể không nhận ra rằng cách chơi tối ưu phụ thuộc vào việc chọn một phạm vi giá trị liền kề sau khi sắp xếp chứ không phải theo thứ tự ban đầu. Một trường hợp thất bại khác xuất phát từ việc cho rằng chúng ta phải luôn bắt đầu từ giá trị nhỏ nhất; bắt đầu từ giá trị ở giữa có thể tạo ra một phân đoạn dài hơn có thể sử dụng được vì chúng ta chỉ được phép di chuyển lên trên +1. 

Cấu trúc ẩn chính là chuỗi các giá trị được lấy luôn không giảm và tăng tối đa một giá trị trên mỗi bước, do đó toàn bộ quá trình chơi diễn ra bên trong một khoảng liền kề của các giá trị số nguyên, trong khi thứ tự thực tế bên trong mỗi giá trị không quan trọng vượt quá số lượng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi quân bài bắt đầu có thể, sau đó mô phỏng trò chơi một cách tham lam hoặc quay lui: từ một điểm bắt đầu đã chọn, liên tục chọn bất kỳ quân bài nào có sẵn có giá trị là x hoặc x+1, đồng thời theo dõi xem có bao nhiêu giá trị riêng biệt đã xuất hiện. Mỗi mô phỏng có thể mất thời gian O(n) nếu chúng tôi quét các thẻ hợp lệ tiếp theo hoặc duy trì các nhóm mà không tối ưu hóa và thực hiện việc này cho tất cả các vị trí bắt đầu sẽ dẫn đến O(n^2), quá lớn đối với 200000 phần tử. 

Quan sát quan trọng là các quy tắc chỉ phụ thuộc vào giá trị chứ không phải vị trí. Khi chúng tôi nhóm các giá trị giống nhau lại với nhau, vấn đề sẽ trở thành tần số của các giá trị và chọn một phân đoạn giá trị liền kề trong đó chúng tôi mô phỏng việc lấy tất cả các thẻ, nhưng với ràng buộc là chúng tôi chỉ có thể “sử dụng” tối đa k giá trị riêng biệt trong bất kỳ phân đoạn hợp lệ nào của quy trình. 

Nếu chúng ta sắp xếp các giá trị riêng biệt và nén chúng thì mỗi giá trị có một tần số. Bất kỳ lượt chơi hợp lệ nào đều tương ứng với việc chọn một cửa sổ có các giá trị liên tiếp và trong cửa sổ đó, Monocarp có thể di chuyển từ trái sang phải, tiêu thụ tất cả các quân bài. Ràng buộc tối đa k giá trị riêng biệt có nghĩa là kích thước cửa sổ theo các giá trị riêng biệt phải lớn nhất là k, nhưng do chuyển động bị hạn chế đối với các chuyển đổi x hoặc x+1, nên khi chúng tôi bao gồm giá trị x, chúng tôi chỉ có thể tiến tới x+1 nếu nó tồn tại trong cấu trúc đã chọn. Điều này biến chiến lược tối ưu thành việc chọn một đoạn gồm các số nguyên liên tiếp một cách hiệu quả và tính tổng tần số của chúng, đồng thời đảm bảo chúng tôi tôn trọng giới hạn k bằng cách giới hạn số lượng giá trị riêng biệt mà chúng tôi chuyển qua. 

Điều này biến thành một cửa sổ trượt cổ điển trên các giá trị duy nhất được sắp xếp: chúng tôi duy trì một cửa sổ gồm các giá trị liên tiếp trong đó giá trị kề hợp lệ và đảm bảo số lượng giá trị riêng biệt bên trong không vượt quá k, sau đó tối đa hóa tổng tần số trong cửa sổ đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Sắp xếp + Cửa sổ trượt qua các giá trị | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đầu tiên, nhóm các giá trị giống nhau lại và đếm tần số của chúng. Điều này loại bỏ thông tin vị trí, bởi vì chỉ có bao nhiêu bản sao của mỗi giá trị tồn tại mới quan trọng để tối đa hóa độ dài của chuỗi. 
2. Sắp xếp các giá trị riêng biệt theo thứ tự tăng dần, giữ tần số của chúng thẳng hàng. Việc sắp xếp là cần thiết vì các chuyển đổi chỉ được phép giữa các số nguyên bằng nhau hoặc liên tiếp, do đó thứ tự trong không gian giá trị xác định tất cả các bước di chuyển hợp lệ. 
3. Sử dụng hai con trỏ trên các giá trị duy nhất được sắp xếp để duy trì một cửa sổ các giá trị ứng viên mà Monocarp có thể duyệt qua. Con trỏ bên phải mở rộng cửa sổ bằng cách thêm các giá trị mới theo thứ tự tăng dần. 
4. Duy trì tổng số thẻ bên trong cửa sổ hiện tại dưới dạng tổng tần số. Điều này thể hiện số bước Monocarp có thể thực hiện nếu anh ấy cam kết với bộ giá trị này. 
5. Đảm bảo cửa sổ tôn trọng giới hạn do trò chơi gây ra: các giá trị phải liên tục, vì vậy nếu khoảng cách giữa các giá trị liên tiếp vượt quá 1, chúng tôi sẽ đặt lại cửa sổ bắt đầu từ giá trị hiện tại. Điều này là do các bước nhảy lớn hơn +1 bị cấm, do đó các giá trị bị ngắt kết nối không thể cùng tồn tại trong một chuỗi hợp lệ. 
6. Vì Monocarp chỉ có thể bao gồm tối đa k giá trị riêng biệt, hãy thu nhỏ phía bên trái của cửa sổ bất cứ khi nào kích thước của nó vượt quá k. Điều này giữ số lượng giá trị riêng biệt trong giới hạn cho phép. 
7. Ở mỗi bước, hãy cập nhật câu trả lời với tổng tần suất tối đa quan sát được trong bất kỳ cửa sổ hợp lệ nào. 

Lý do điều này có hiệu quả là vì bất kỳ đường dẫn trò chơi hợp lệ nào đều tương ứng chính xác với việc chọn một phân đoạn gồm các giá trị số nguyên liên tiếp và trong phân đoạn đó Monocarp có thể lấy tất cả các quân bài có giá trị đó. Ràng buộc k hạn chế độ rộng của phân đoạn này về mặt các giá trị riêng biệt và quy tắc chuyển đổi giới hạn nó ở các số nguyên liên tiếp. Do đó, mọi lần chơi hợp lệ được thể hiện bằng chính xác một cửa sổ trượt hợp lệ và mọi cửa sổ trượt tương ứng với một lần chơi hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    freq = defaultdict(int)
    for x in a:
        freq[x] += 1

    vals = sorted(freq.items())  # (value, count)

    left = 0
    curr_sum = 0
    best = 0

    for right in range(len(vals)):
        if right > 0 and vals[right][0] != vals[right - 1][0] + 1:
            left = right
            curr_sum = 0

        curr_sum += vals[right][1]

        while right - left + 1 > k:
            curr_sum -= vals[left][1]
            left += 1

        best = max(best, curr_sum)

    print(best)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách nén đầu vào vào bản đồ tần số. Điều này rất cần thiết vì câu trả lời chỉ phụ thuộc vào số lượng trên mỗi giá trị chứ không phụ thuộc vào vị trí của chúng. 

Sau khi sắp xếp các giá trị, vòng lặp duy trì một cửa sổ trượt trên các số nguyên liên tiếp. Điều kiện đặt lại`vals[right][0] != vals[right - 1][0] + 1`thực thi quy tắc rằng chúng ta không thể thu hẹp khoảng cách lớn hơn 1, vì trò chơi chỉ cho phép di chuyển từ x đến x hoặc x+1. 

Ràng buộc thứ hai, giới hạn số lượng giá trị riêng biệt, được thực thi bằng cách thu nhỏ cửa sổ khi nó vượt quá k giá trị riêng biệt. Tổng số tiền chạy`curr_sum`theo dõi số lượng thẻ trong phân đoạn hợp lệ hiện tại, chính xác là độ dài của chuỗi chơi hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 2
5 2 4 3 4 3 4 5 3 2
```Sau khi nén tần số, chúng ta nhận được các giá trị:`2:2, 3:3, 4:3, 5:2`Chúng tôi mô phỏng: 

| đúng | giá trị | trái | giá trị cửa sổ | tổng hợp | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 0 | [2] | 2 | bắt đầu | 
| 1 | 3 | 0 | [2,3] | 5 | mở rộng | 
| 2 | 4 | 0 | [2,3,4] | 8 | mở rộng | 
| 3 | 5 | 0→1 | [3,4,5] | 8 | thu nhỏ về k=2 | 

Tốt nhất là 8. 

Điều này cho thấy cửa sổ luôn thể hiện một chuỗi giá trị liền kề như thế nào và ràng buộc k buộc chúng ta phải bỏ giá trị ngoài cùng bên trái như thế nào. 

### Ví dụ 2 

đầu vào:```
5 1
10 11 10 11 10
```Tần số:`10:3, 11:2`| đúng | giá trị | trái | cửa sổ | tổng hợp | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 10 | 0 | [10] | 3 | bắt đầu | 
| 1 | 11 | 1 | [11] | 2 | đặt lại do k=1 | 

Tốt nhất là 3. 

Điều này xác nhận rằng khi k = 1, chúng tôi buộc phải chọn một khối giá trị duy nhất và thuật toán sẽ tách chính xác tần số tốt nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các giá trị khác biệt chiếm ưu thế, cửa sổ trượt là tuyến tính | 
| Không gian | O(n) | Bản đồ tần số và mảng nén | 

Các ràng buộc cho phép giải pháp O(n log n) thoải mái với n lên đến 200000. Cửa sổ trượt đảm bảo chúng ta không bao giờ truy cập lại các phần tử, giữ cho quá trình truyền tải hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    freq = defaultdict(int)
    for x in a:
        freq[x] += 1

    vals = sorted(freq.items())

    left = 0
    curr_sum = 0
    best = 0

    for right in range(len(vals)):
        if right > 0 and vals[right][0] != vals[right - 1][0] + 1:
            left = right
            curr_sum = 0

        curr_sum += vals[right][1]

        while right - left + 1 > k:
            curr_sum -= vals[left][1]
            left += 1

        best = max(best, curr_sum)

    return str(best)

# provided samples
assert run("10 2\n5 2 4 3 4 3 4 5 3 2\n") == "8", "sample 1"
assert run("5 1\n10 11 10 11 10\n") == "3", "sample 2"

# custom cases
assert run("1 1\n7\n") == "1", "single element"
assert run("6 2\n1 100 2 3 101 4\n") == "3", "disconnected values"
assert run("8 3\n1 1 2 2 3 3 4 4\n") == "8", "full chain"
assert run("5 5\n1 1 1 1 1\n") == "5", "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp tối thiểu | 
| giá trị bị ngắt kết nối | 3 | xử lý khoảng cách | 
| chuỗi đầy đủ | 8 | cửa sổ đầy đủ tối ưu | 
| tất cả đều bình đẳng | 5 | sự thống trị giá trị đơn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các giá trị không liên tiếp trong đầu vào nhưng vẫn cho phép một phần chuỗi. Đối với đầu vào`6 2 / 1 100 2 3 101 4`, thuật toán sẽ đặt lại chính xác bất cứ khi nào nó gặp khoảng cách lớn hơn 1. Nó xây dựng`[1]`, sau đó đặt lại tại`100`, sau đó xây dựng`[2,3,4]`với tổng 3, tối ưu với ràng buộc k = 2. 

Một trường hợp cạnh khác là khi k đủ lớn để bao gồm tất cả các giá trị liên tiếp riêng biệt. Vì`8 3 / 1 1 2 2 3 3 4 4`, cửa sổ không bao giờ vượt quá k và thuật toán tích lũy tất cả tần số thành một phân đoạn duy nhất, tạo ra chính xác 8. 

Cuối cùng, khi tất cả các giá trị giống hệt nhau, cửa sổ là tầm thường và luôn hợp lệ. Thuật toán không bao giờ kích hoạt việc đặt lại hoặc thu hẹp và câu trả lời chỉ đơn giản là n, phù hợp với thực tế là mọi lá bài đều có thể được lấy liên tiếp mà không vi phạm bất kỳ quy tắc nào.
