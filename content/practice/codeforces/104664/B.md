---
title: "CF 104664B - Mì kéo co"
description: "Chúng ta được cho một dãy các số nguyên dương biểu thị sức mạnh của những người tham gia được sắp xếp thành một hàng. Nhiệm vụ là chọn một vị trí phân chia duy nhất sao cho mảng được chia thành tiền tố bên trái và hậu tố bên phải."
date: "2026-06-29T10:02:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104664
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 10-06-23 Div. 2 (Beginner)"
rating: 0
weight: 104664
solve_time_s: 53
verified: true
draft: false
---

[CF 104664B - Mỳ kéo co](https://codeforces.com/problemset/problem/104664/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các số nguyên dương biểu thị sức mạnh của những người tham gia được sắp xếp thành một hàng. Nhiệm vụ là chọn một vị trí phân chia duy nhất sao cho mảng được chia thành tiền tố bên trái và hậu tố bên phải. Điểm của một lượt chia được định nghĩa là tích của tổng sức mạnh ở bên trái và tổng sức mạnh ở bên phải. Mục tiêu là tìm ra cách phân chia tối đa hóa sản phẩm này. 

Nói cách khác, nếu chúng ta biểu thị một tiền tố có tổng tới chỉ số k là$S_k$, thì điểm ở lần chia k là$S_k \cdot (S_N - S_k)$. Chúng tôi muốn giá trị tối đa trên tất cả k hợp lệ từ 1 đến N-1. Trường hợp đặc biệt khi N bằng 1 là tầm thường vì một bên trống và tích bằng 0. 

Các ràng buộc cho phép lên đến$10^5$các phần tử, mỗi phần tử lên tới$10^4$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận bậc hai nào đối với các vị trí được phân chia kết hợp với việc tính toán lại tổng. Một giải pháp thử mọi phép chia và tính toán lại cả hai bên từ đầu sẽ làm được khoảng$O(N^2)$bổ sung, quá chậm so với thời hạn. 

Trường hợp cạnh phổ biến là khi tất cả các phần tử đều bằng nhau. Ví dụ: nếu mảng là [1, 1, 1, 1] thì cách phân chia tốt nhất là ở giữa tạo ra 2 và 2, cho kết quả 4. Một cách tiếp cận ngây thơ là quản lý sai các tổng tiền tố có thể tính toán lại một phần phạm vi một cách không chính xác hoặc bỏ lỡ phần phân chia cân bằng tối ưu. 

Một trường hợp tinh tế khác là N = 1. Đối với đầu vào [x], mọi phép phân chia đều không hợp lệ theo nghĩa thông thường, nhưng vấn đề xác định câu trả lời là 0. Việc triển khai bất cẩn luôn đánh giá k từ 1 đến N có thể cố gắng tính toán sai cả hai bên hoặc truy cập vào một hậu tố trống mà không xử lý nó một cách rõ ràng. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mọi vị trí phân chia có thể k, hãy tính tổng các phần tử từ 1 đến k và tổng từ k+1 đến N, sau đó nhân chúng. Mỗi phép tính tổng có thể được thực hiện bằng cách quét mảng, do đó mỗi lần chia chi phí$O(N)$. Vì có$O(N)$chia tách, tổng độ phức tạp trở thành$O(N^2)$. Với$N = 10^5$, điều này sẽ liên quan đến khoảng$10^{10}$hoạt động vượt quá giới hạn cho phép. 

Quan sát quan trọng là việc tính toán lại các khoản tiền nhiều lần là lãng phí. Khi chúng ta biết tổng của mảng, tổng bên phải của phép chia có thể được suy ra từ tổng bên trái trong thời gian không đổi. Nếu chúng ta duy trì tổng tiền tố đang chạy trong khi lặp qua mảng thì mỗi đánh giá phân tách sẽ trở thành$O(1)$. Điều này làm giảm vấn đề từ việc tính toán lại phạm vi đến việc duy trì một trạng thái tích lũy duy nhất. 

Vì vậy, thay vì tính lại tổng, chúng tôi tính tổng một lần, sau đó duyệt qua mảng, cập nhật tổng tiền tố. Tại mỗi vị trí, tổng bên phải chỉ đơn giản là tiền tố tổng trừ. Chúng tôi tính toán sản phẩm và theo dõi mức tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^2) | O(1) | Quá chậm | 
| Quét tổng tiền tố | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng các phần tử trong mảng. Điều này thể hiện tổng của cả hai bên cộng lại trước khi bất kỳ sự phân chia nào được chọn. 
2. Khởi tạo một biến`prefix`về không. Điều này sẽ lưu trữ tổng của phía bên trái khi chúng ta di chuyển điểm phân chia từ trái sang phải. 
3. Khởi tạo một biến`best`về không. Điều này lưu trữ sản phẩm tối đa được tìm thấy cho đến nay. 
4. Lặp lại mảng từ phần tử đầu tiên đến phần tử thứ hai đến phần tử cuối cùng. Chúng tôi dừng lại trước phần tử cuối cùng vì phía bên phải không được trống để phân chia có ý nghĩa. 
5. Tại mỗi chỉ mục i, thêm phần tử hiện tại vào`prefix`. Điều này mở rộng nhóm bên trái bằng cách bao gồm người tham gia hiện tại. 
6. Tính tổng vế phải như sau`total - prefix`. Điều này hiệu quả vì mọi phần tử đều ở tiền tố hoặc hậu tố, do đó phép trừ sẽ cho ra tổng chính xác còn lại. 
7. Tính điểm`prefix * (total - prefix)`và cập nhật`best`nếu giá trị này lớn hơn mức tối đa hiện tại. 

Sau khi hoàn thành việc lặp lại,`best`giữ số điểm tối đa có thể. 

Tính chính xác dựa trên thực tế là mọi phép chia hợp lệ đều tương ứng với chính xác một giá trị tổng tiền tố. Bằng cách lặp lại tất cả các tiền tố, chúng tôi liệt kê tất cả các phân vùng có thể có và đối với mỗi phân vùng, chúng tôi tính toán điểm chính xác mà không tính gần đúng hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    if n == 1:
        print(0)
        return

    total = sum(a)
    prefix = 0
    best = 0

    for i in range(n - 1):
        prefix += a[i]
        right = total - prefix
        best = max(best, prefix * right)

    print(best)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xử lý trường hợp một phần tử một cách rõ ràng, vì không có sự phân chia hợp lệ nào tồn tại. Tổng số tiền được tính một lần và được sử dụng lại trong suốt quá trình quét. Vòng lặp dừng ở`n-1`để đảm bảo hậu tố không trống. 

Một chi tiết tinh tế là thứ tự cập nhật: chúng tôi thêm phần tử vào`prefix`trước khi tính tích, căn chỉnh chỉ số i với phần tách sau khi bao gồm a[i]. Việc trộn thứ tự này sẽ làm dịch chuyển phần tách và tạo ra kết quả không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 4 5
```| tôi | tiền tố | đúng | sản phẩm | 
| --- | --- | --- | --- | 
| 0 | 1 | 14 | 14 | 
| 1 | 3 | 12 | 36 | 
| 2 | 6 | 9 | 54 | 
| 3 | 10 | 5 | 50 | 

Mức tối đa xảy ra khi phần phân tách nằm sau phần tử thứ ba. Tổng tiền tố là 6 và tổng hậu tố là 9, cho kết quả 54. Điều này cho thấy cách phân chia tốt nhất có xu hướng cân bằng hai bên hơn là thiên về sự phân chia cực đoan. 

### Ví dụ 2 

đầu vào:```
4
3 2 5 1
```| tôi | tiền tố | đúng | sản phẩm | 
| --- | --- | --- | --- | 
| 0 | 3 | 8 | 24 | 
| 1 | 5 | 6 | 30 | 
| 2 | 10 | 1 | 10 | 

Cách phân chia tốt nhất là sau phần tử thứ hai, tạo ra tiền tố 5 và hậu tố 6, mang lại 30. Điều này xác nhận rằng cách phân chia tối ưu không nhất thiết phải tập trung vào chỉ số mà phụ thuộc vào tổng tích lũy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Một lượt để tính tổng và một lượt để đánh giá tất cả các phần chia | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được duy trì | 

Độ phức tạp tuyến tính là đủ cho$N = 10^5$, vì nó chỉ thực hiện một lần truyền qua mảng đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    if n == 1:
        return "0\n"

    total = sum(a)
    prefix = 0
    best = 0

    for i in range(n - 1):
        prefix += a[i]
        best = max(best, prefix * (total - prefix))

    return str(best) + "\n"

# provided samples
assert run("5\n1 2 3 4 5\n") == "54\n"
assert run("4\n3 2 5 1\n") == "30\n"

# custom cases
assert run("1\n7\n") == "0\n", "single element"
assert run("2\n10 10\n") == "100\n", "minimal split"
assert run("5\n1 1 1 1 1\n") == "6\n", "uniform array"
assert run("6\n5 4 3 2 1 0\n") == "54\n", "descending with zero"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | trường hợp cạnh đơn phần tử | 
| 10 10 | 100 | phép chia nhỏ nhất có ý nghĩa | 
| tất cả những cái | 6 | sự phân chia cân bằng đúng đắn | 
| giảm dần + không | 54 | xử lý phân phối không đồng đều | 

## Vỏ cạnh 

Đối với đầu vào`1 7`, không có phép chia hợp lệ. Thuật toán phát hiện`n == 1`và ngay lập tức trả về 0 mà không cần vào vòng lặp. Điều này tránh mọi tính toán không chính xác liên quan đến hậu tố trống. 

Vì`10 10`, tổng cộng là 20. Tại i = 0, tiền tố trở thành 10 và hậu tố cũng là 10, tạo ra 100. Vì chỉ có một phép chia nên thuật toán đánh giá chính xác một ứng cử viên. 

Vì`[1, 1, 1, 1, 1]`, tổng cộng là 5. Tiến hóa tiền tố tạo ra các sản phẩm 1×4, 2×3, 3×2, 4×1. Tối đa là 6 ở phần phân chia ở giữa và quá trình quét sẽ đánh giá mọi phần phân chia có thể có chính xác một lần, đảm bảo không thiếu trường hợp nào. 

Vì`[5, 4, 3, 2, 1, 0]`, sự hiện diện của số 0 không ảnh hưởng đến tính đúng đắn. Sự phân chia tối ưu xảy ra trước khi hậu tố nhỏ hơn chiếm ưu thế. Việc phân tách tiền tố-hậu tố vẫn được giữ vì số 0 được bao gồm trong tổng và công thức trừ vẫn hợp lệ cho mọi vị trí.
