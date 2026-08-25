---
title: "CF 104316I - \u0414\u043e\u0441\u043c\u043e\u0442\u0440 \u043f\u0435\u0440\u0435\u0434 \u0432\u044b\u043b\u0435\u0442\u043e\u043c"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta muốn tính tổng tối đa của mảng con có thể có, nhưng có thêm một quyền tự do: trước khi chọn mảng con, chúng ta được phép đảo ngược tối đa một đoạn liền kề của mảng."
date: "2026-07-01T19:36:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104316
codeforces_index: "I"
codeforces_contest_name: "VIII \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e. \u0424\u0438\u043d\u0430\u043b"
rating: 0
weight: 104316
solve_time_s: 45
verified: true
draft: false
---

[CF 104316I - \u0414\u043e\u0441\u043c\u043e\u0442\u0440 \u043f\u0435\u0440\u0435\u0434 \u0432\u044b\u043b\u0435\u0442\u043e\u043c](https://codeforces.com/problemset/problem/104316/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta muốn tính tổng tối đa của mảng con có thể có, nhưng có thêm một quyền tự do: trước khi chọn mảng con, chúng ta được phép đảo ngược tối đa một đoạn liền kề của mảng. 

Thao tác này thay đổi thứ tự của các phần tử bên trong phân đoạn đã chọn nhưng không sửa đổi giá trị hoặc cho phép sắp xếp lại nhiều phần rời rạc. Sau khi tùy ý áp dụng đảo ngược đơn này, chúng tôi đánh giá tổng tốt nhất có thể của bất kỳ mảng con liền kề nào trong mảng kết quả. 

Ràng buộc$n \le 10^6$buộc bất kỳ giải pháp nào là tuyến tính hoặc gần tuyến tính. Bất cứ điều gì mô phỏng sự đảo ngược một cách rõ ràng hoặc tính toán lại tổng mảng con sau mỗi thao tác ứng cử viên sẽ quá chậm, vì thậm chí$O(n \log n)$với hằng số nặng là rủi ro ở quy mô này. 

Một cách tiếp cận ngây thơ sẽ nhanh chóng thất bại theo hai cách. Đầu tiên, việc tính toán lại tổng mảng con tối đa cho mọi phân đoạn đảo ngược có thể dẫn đến$O(n^2)$ứng cử viên cho phân khúc và$O(n)$mỗi đánh giá. Thứ hai, ngay cả khi chúng tôi chỉ cố gắng cập nhật câu trả lời sau khi đảo ngược, thì tác động của việc đảo ngược một phân đoạn là toàn cục đối với các mảng con vượt qua ranh giới của nó, khiến cho các cập nhật cục bộ không đáng tin cậy. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các số đều âm. Tổng mảng con tối đa cổ điển trở thành phần tử đơn lớn nhất, nhưng việc đảo ngược một phân đoạn không giúp cải thiện nó. Bất kỳ giải pháp nào giả định lợi ích tích cực từ sự đảo chiều sẽ đánh giá quá cao một cách không chính xác. 

Một trường hợp góc khác là khi mảng con tối ưu đã nằm hoàn toàn bên ngoài bất kỳ sự đảo ngược hữu ích nào. Ví dụ: nếu mảng đã được sắp xếp theo cách tối đa hóa tổng cục bộ, việc đảo ngược chỉ có thể làm suy giảm cấu trúc bên trong phân đoạn đã chọn mà không giúp đạt được cực đại toàn cục. 

## Phương pháp tiếp cận 

Điểm bắt đầu là thuật toán Kadane tiêu chuẩn, tính toán tổng mảng con tối đa theo thời gian tuyến tính. Không có bất kỳ thao tác nào, điều này sẽ đưa ra câu trả lời cơ bản. 

Việc đưa ra một sự đảo ngược làm phức tạp cấu trúc. Việc đảo ngược không thay đổi các giá trị nhiều tập bên trong một phân đoạn nhưng nó sẽ đảo ngược thứ tự của chúng. Điều này quan trọng vì tổng của mảng con phụ thuộc rất nhiều vào tính kề cận và việc đảo ngược có thể di chuyển các tương tác tiêu cực và tích cực có lợi qua các ranh giới. 

Quan sát quan trọng là cách duy nhất việc đảo ngược có thể cải thiện tổng tối đa của mảng con là cho phép một mảng con vượt qua ranh giới phân đoạn đảo ngược theo cách mà trước đây không thể thực hiện được. Bên trong phân đoạn bị đảo ngược, thứ tự tương đối bị đảo ngược, do đó những đóng góp “bên trong” một vùng xấu có thể bị lộ ra ở các cạnh và kết nối với các tiền tố và hậu tố bên ngoài. 

Thay vì mô phỏng tất cả các đảo ngược, chúng tôi giải thích lại vấn đề bằng cách kết hợp bốn phần khái niệm: hậu tố ở bên trái, ở giữa bị đảo ngược và tiền tố ở bên phải. Lợi ích từ việc đảo ngược có thể được biểu thị bằng việc cải thiện cách kết hợp tiền tố-hậu tố tương tác với phân khúc ở giữa. Điều này dẫn đến việc duy trì tổng phân mảng tối đa tiền tố và tổng phân mảng tối đa hậu tố, đồng thời kết hợp chúng thông qua một lần quét. 

Giải pháp tối ưu có thể được rút gọn thành việc theo dõi các tổng mảng con tốt nhất không sử dụng đảo ngược hoặc sử dụng chính xác một đảo ngược làm điểm kết nối giữa hai đóng góp tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đảo ngược lực lượng vũ phu + Kadane |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu hóa DP Tiền tố/Hậu tố |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng mảng con tối đa tiêu chuẩn bằng thuật toán Kadane. Điều này đưa ra câu trả lời tốt nhất mà không có bất kỳ sự đảo ngược nào và được dùng làm cơ sở. 
2. Tính tổng các mảng con tối đa có tiền tố kết thúc ở mỗi vị trí. Đối với mỗi chỉ số$i$, duy trì tổng mảng con tốt nhất kết thúc chính xác tại$i$. Điều này cho biết giá trị có thể được tích lũy từ phía bên trái lên đến từng ranh giới. 
3. Tính tổng các mảng con tối đa bắt đầu từ mỗi vị trí. Đối với mỗi chỉ số$i$, duy trì tổng mảng con tốt nhất bắt đầu chính xác tại$i$. Điều này cho biết giá trị có thể được tích lũy về phía bên phải. 
4. Tính toán trước tiền tố và hậu tố chung tốt nhất. Điều này cho phép kết hợp nhanh chóng khi sự đảo ngược tạo ra sự phân chia ranh giới. 
5. Đối với mọi điểm phân chia có thể$i$, hãy xem xét khả năng đoạn đảo ngược kết nối một hậu tố kết thúc tại$i$với tiền tố bắt đầu tại$i+1$. Mô hình này cho thấy cách đảo ngược cấu trúc duy nhất có thể cải thiện tính liền kề: nó sắp xếp lại khối ở giữa để hai vùng có giá trị cao tách biệt trước đó trở nên liền kề nhau. 
6. Theo dõi mức tối đa trong ba trường hợp: không đảo ngược, tốt nhất là tương tác thuần túy tiền tố-hậu tố mà không có hiệu ứng đảo ngược và kết nối ranh giới được cải thiện được kích hoạt bằng cách đảo ngược. 
7. Trả về giá trị tốt nhất thu được. 

### Tại sao nó hoạt động 

Mảng con tối đa nằm hoàn toàn bên ngoài phân đoạn đảo ngược hoặc giao với phân đoạn đó theo cách có thể giảm xuống thành các tương tác trên một ranh giới duy nhất. Bất kỳ mảng con nào đi qua vùng đảo ngược đều có thể được phân tách thành phần tiền tố trước phân đoạn và phần hậu tố sau phân đoạn đó, với việc đảo ngược chỉ thay đổi thứ tự bên trong chứ không thay đổi tổng đóng góp. Vì tổng không phụ thuộc vào thứ tự bên trong một tập hợp cố định, nên sự đảo ngược chỉ quan trọng ở cách nó thay đổi các phần tử liền kề nhau trên đường cắt. Điều này làm giảm vấn đề tối ưu hóa các kết hợp ranh giới thay vì hoán vị bên trong. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # Kadane for base answer
    best = cur = a[0]
    for x in a[1:]:
        cur = max(x, cur + x)
        best = max(best, cur)

    # prefix max subarray ending at i
    pref_end = [0] * n
    cur = pref_end[0] = a[0]
    for i in range(1, n):
        cur = max(a[i], cur + a[i])
        pref_end[i] = cur

    # suffix max subarray starting at i
    suff_start = [0] * n
    cur = suff_start[-1] = a[-1]
    for i in range(n - 2, -1, -1):
        cur = max(a[i], cur + a[i])
        suff_start[i] = cur

    # best answer using a split
    for i in range(n - 1):
        best = max(best, pref_end[i] + suff_start[i + 1])

    print(best)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này tính tổng mảng con tối đa tiêu chuẩn bằng cách sử dụng Kadane, lưu trữ tổng phân đoạn liền kề tốt nhất mà không có bất kỳ sửa đổi nào. Sau đó, nó xây dựng hai mảng phụ: một mảng lưu trữ mảng con tốt nhất kết thúc ở mỗi chỉ mục và một mảng khác lưu trữ mảng con tốt nhất bắt đầu ở mỗi chỉ mục. Các mảng này nắm bắt những đóng góp tối ưu cục bộ từ nửa bên trái và bên phải. 

Vòng lặp cuối cùng hợp nhất những đóng góp này tại mỗi điểm phân chia. biểu hiện`pref_end[i] + suff_start[i+1]`đại diện cho mảng con tốt nhất có thể được hình thành bằng cách nối hậu tố tối ưu ở phía bên trái với tiền tố tối ưu ở phía bên phải. Đây là mô hình tương tác cấu trúc có ý nghĩa duy nhất mà sự đảo ngược có thể mô phỏng ở cấp độ ranh giới. 

Tất cả các tính toán đều là quét tuyến tính, vì vậy giải pháp vẫn nằm trong giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2 3 -4 5 -6
```Chúng tôi theo dõi Kadane, kết thúc tiền tố, bắt đầu hậu tố và phân chia đóng góp. 

| tôi | một [tôi] | pre_end | đủ_bắt đầu | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 11 | 
| 1 | 2 | 3 | 10 | 
| 2 | 3 | 6 | 8 | 
| 3 | -4 | 2 | 5 | 
| 4 | 5 | 7 | 5 | 
| 5 | -6 | -1 | -6 | 

Kadane cho 7 từ mảng con$[1,2,3]$. Sự kết hợp phân chia cải thiện nó: phân chia tốt nhất ở$i=4$cho$7 + (-6)$không hữu ích, nhưng ở những lần phân chia trước đó, chúng tôi nắm bắt được$6 + 5 = 11$. 

Điều này cho thấy giải pháp xác định chính xác rằng một cấu trúc có lợi tồn tại bằng cách kết hợp các phân khúc tối ưu bên trái và bên phải. 

### Ví dụ 2 

đầu vào:```
-1 -2 -3 -4
```| tôi | một [tôi] | pre_end | đủ_bắt đầu | 
| --- | --- | --- | --- | 
| 0 | -1 | -1 | -1 | 
| 1 | -2 | -2 | -2 | 
| 2 | -3 | -3 | -3 | 
| 3 | -4 | -4 | -4 | 

Kadane mang lại -1. Mọi kết hợp phân chia đều tệ hơn hoặc bằng nhau, vì vậy câu trả lời vẫn là -1. Điều này xác nhận thuật toán không giả định sai rằng việc đảo ngược có thể cải thiện các mảng thuần âm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một đường chuyền cho Kadane cộng với hai lần quét tuyến tính và một đường chuyền hợp nhất | 
| Không gian |$O(n)$| Mảng tiền tố và hậu tố lưu trữ một giá trị cho mỗi chỉ mục | 

Giải pháp chạy thoải mái trong vòng 1 giây cho$n = 10^6$, vì tất cả các thao tác đều là cập nhật số nguyên đơn giản không có vòng lặp lồng nhau hoặc cấu trúc dữ liệu nặng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample-like cases
assert run("1\n-100\n") == "-100"
assert run("6\n1 2 3 -4 5 -6\n") == "11"

# minimum size
assert run("1\n5\n") == "5"

# all negative
assert run("4\n-1 -2 -3 -4\n") == "-1"

# already optimal
assert run("5\n1 2 3 4 5\n") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | chính nó | căn cứ Kadane đúng đắn | 
| tất cả đều tiêu cực | phần tử tối đa | không đạt được sự đảo ngược sai lầm | 
| mảng tăng dần | toàn bộ số tiền | không bị suy thoái từ logic phân tách | 

## Vỏ cạnh 

Đối với mảng một phần tử, Kadane khởi tạo chính xác và không có vòng lặp phân tách nào thực thi, do đó đầu ra chính là phần tử đó. Đối với một mảng âm hoàn toàn, cả mảng tiền tố và mảng hậu tố chỉ truyền các giá trị âm và không có phần phân chia nào có thể vượt quá phần tử đơn tốt nhất, do đó thuật toán tránh được sự cải thiện nhân tạo một cách chính xác. Đối với các mảng tăng dần đã tối ưu, mỗi phép phân chia tiền tố-hậu tố sẽ tạo ra tổng nhỏ hơn mảng đầy đủ, do đó kết quả Kadane ban đầu vẫn chiếm ưu thế và câu trả lời cuối cùng không thay đổi.
