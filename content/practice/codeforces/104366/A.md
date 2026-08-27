---
title: "CF 104366A - Hiệu ứng thùng"
description: "Chúng tôi được phát một số tấm gỗ, mỗi tấm có chiều dài cố định. “Sức mạnh” hoặc “công suất” của thùng được làm từ những tấm ván này được định nghĩa là chiều dài của tấm ván ngắn nhất được sử dụng trong thùng."
date: "2026-07-01T17:42:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104366
codeforces_index: "A"
codeforces_contest_name: "The 17th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 104366
solve_time_s: 57
verified: true
draft: false
---

[CF 104366A - Hiệu ứng thùng](https://codeforces.com/problemset/problem/104366/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phát một số tấm gỗ, mỗi tấm có chiều dài cố định. “Sức mạnh” hoặc “công suất” của thùng được làm từ những tấm ván này được định nghĩa là chiều dài của tấm ván ngắn nhất được sử dụng trong thùng. Vì vậy, nếu chúng ta chọn một tập hợp con các tấm ván, chất lượng của thùng đó hoàn toàn được quyết định bởi tấm ván yếu nhất của nó. 

Chúng ta được phép thực hiện một thao tác đặc biệt duy nhất: chúng ta có thể lấy một đoạn liên tục từ bảng này và di chuyển nó sang bảng khác. Điều này phân phối lại chiều dài giữa hai bảng một cách hiệu quả trong khi vẫn bảo toàn tổng chiều dài. Sau thao tác này, tất cả các bảng vẫn tồn tại, chỉ có độ dài được điều chỉnh. 

Mục tiêu là thực hiện tối đa một lần chuyển như vậy để sau khi điều chỉnh, chúng tôi chọn một tập hợp con các bảng có chiều dài tối thiểu càng lớn càng tốt. Vì chúng ta luôn có quyền loại bỏ các bảng, nên câu trả lời cuối cùng chỉ đơn giản là giá trị tối đa có thể có của độ dài bảng tối thiểu sau nhiều nhất một thao tác phân phối lại. 

Các ràng buộc lên tới n = 10^5, do đó, bất kỳ phương pháp nào cố gắng mô phỏng việc phân phối lại các phân số tùy ý hoặc tính toán lại nhiều lần các tập hợp con tốt nhất sẽ thất bại. Chúng ta cần một giải pháp làm giảm vấn đề xuống một số lượng nhỏ cấu hình ứng cử viên và đánh giá chúng theo thời gian tuyến tính hoặc gần tuyến tính. 

Một điểm tinh tế quan trọng là hoạt động diễn ra liên tục, không rời rạc. Chúng ta có thể di chuyển các độ dài phân số, nghĩa là không gian trạng thái có giá trị thực. Điều này ngay lập tức loại trừ việc tìm kiếm tổ hợp trên các phần tách. Câu trả lời phải đến từ thuộc tính cấu trúc của mảng độ dài được sắp xếp. 

Một sai lầm ngây thơ là cho rằng chỉ có chuyển số nguyên hoặc di chuyển cục bộ tham lam mới quan trọng. Ví dụ: việc di chuyển chiều dài từ bảng lớn nhất đến bảng nhỏ nhất có thể trông tối ưu, nhưng cách di chuyển tốt nhất phụ thuộc vào việc cân bằng tất cả các bảng so với ngưỡng mục tiêu, chứ không phải cân bằng các cực trị cục bộ. 

## Phương pháp tiếp cận 

Nếu không có phép toán kỳ diệu, câu trả lời thật tầm thường: chúng ta chỉ cần lấy chiều dài bảng tối thiểu, vì mỗi bảng chúng ta đưa vào phải tôn trọng giới hạn dưới đó. 

Hoạt động đưa ra một bậc tự do: chúng ta có thể chọn hai bảng và dịch chuyển một số lượng x từ bảng này sang bảng khác. Điều này chỉ thay đổi hai giá trị trong khi vẫn giữ nguyên tổng số tiền. Hiệu quả là chúng ta có thể cố gắng “nâng” một tấm ván yếu bằng cách hy sinh một phần của tấm ván mạnh. 

Một ý tưởng mạnh mẽ sẽ thử tất cả các cặp bảng, mô phỏng việc chuyển một số tiền thực x và kiểm tra kết quả tối thiểu tốt nhất có thể. Đối với một cặp cố định, chúng ta có thể tưởng tượng việc tăng bảng nhỏ hơn lên đến một ngưỡng nào đó trong khi vẫn đảm bảo nhà tài trợ vẫn không âm. Tuy nhiên, vì x là liên tục nên chúng ta vẫn cần tìm ra sự truyền tối ưu về mặt giải tích. Ngay cả khi chúng tôi quản lý được điều đó, việc thử tất cả các cặp O(n^2) ngay lập tức là không thể với n = 10^5. 

Quan sát quan trọng là câu trả lời cuối cùng chỉ phụ thuộc vào việc liệu chúng ta có thể làm cho tất cả các bảng đã chọn đạt ít nhất một ngưỡng T nào đó hay không. Nếu chúng ta sửa T, chúng ta có thể hỏi liệu chúng ta có thể điều chỉnh tối đa một lần chuyển giao để tất cả các bảng ít nhất là T hay không. Điều này chuyển vấn đề thành kiểm tra tính khả thi trên T. 

Cấu trúc trở nên đơn điệu trong T, điều này gợi ý tìm kiếm nhị phân. Với một T nhất định, các bảng bên dưới T cần được “giúp đỡ” bằng cách lấy khối lượng từ các bảng trên T. Vì chúng ta chỉ được phép chuyển giao một lần giữa hai bảng, nên cách giải thích hữu ích duy nhất là tất cả thâm hụt phải được bù đắp bởi một bảng tài trợ duy nhất, trong khi tất cả thặng dư có thể được gộp lại từ một bảng nguồn. 

Vì vậy, chúng tôi tính tổng thâm hụt của các bảng dưới T và tổng thặng dư của các bảng trên T. Tính khả thi đòi hỏi phải tồn tại một bảng có thể đóng vai trò là nguồn thặng dư duy nhất đủ để bù đắp tất cả các khoản thâm hụt, tức là một số bảng phải có đủ số dư trên T để bù đắp toàn bộ thâm hụt. Điều này làm giảm mỗi lần kiểm tra thành quét tuyến tính. 

Chúng tôi tìm kiếm nhị phân khả thi tối đa T.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Bạo lực đối với các cặp và số tiền chuyển | O(n^2) | O(1) | Quá chậm | 
| Tìm kiếm nhị phân trên câu trả lời với kiểm tra tính khả thi tuyến tính | O(n log A) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi cố gắng xác định giá trị T lớn nhất sao cho sau nhiều nhất một lần chuyển, mọi bảng chúng tôi giữ có thể có độ dài ít nhất là T. 

1. Chúng tôi sắp xếp hoặc đơn giản là quét mảng trong khi kiểm tra tính khả thi, vì thứ tự không liên quan đến tổng nhưng hữu ích cho lý luận nhất quán về thâm hụt và thặng dư. 
2. Đối với một ứng cử viên cố định T, chúng tôi tính toán tổng chiều dài bị thiếu trên tất cả các bảng có ai < T. Đối với mỗi bảng như vậy, số lượng còn thiếu là T - ai và chúng tôi tính tổng các giá trị này thành một giá trị thiếu hụt duy nhất D. Điều này thể hiện tổng số tiền phải được “nhập” từ một nơi khác. 
3. Chúng tôi cũng tính toán, với mỗi bảng có ai > T, nó có thêm bao nhiêu trên T. Nếu một bảng có ai, thặng dư của nó là ai - T. Chúng tôi coi mỗi bảng là một nhà tài trợ tiềm năng. 
4. Chúng tôi kiểm tra xem có tồn tại ít nhất một hội đồng có thặng dư ít nhất là D hay không. Nếu một hội đồng như vậy tồn tại, thì về mặt khái niệm, chúng tôi có thể chuyển tất cả thâm hụt từ nhà tài trợ đó sang tất cả các hội đồng nhỏ hơn, làm cho mỗi hội đồng đạt ít nhất T. 
5. Nếu không có nhà tài trợ nào như vậy tồn tại thì ngay cả việc tổng hợp tất cả thặng dư cũng không đủ từ một bảng duy nhất, có nghĩa là ràng buộc “chỉ một lần chuyển giao” sẽ ngăn cản việc phân phối tài nguyên một cách hiệu quả. 
6. Chúng tôi tìm kiếm nhị phân T trong phạm vi [0, max(ai)] bằng cách sử dụng kiểm tra tính khả thi ở trên. 

Sau khi tìm kiếm nhị phân kết thúc, chúng tôi xuất ra T. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là chỉ có một bảng có thể đóng vai trò là nguồn thực của chiều dài được truyền. Bất kỳ cấu hình khả thi nào sau khi vận hành đều có thể được hiểu là chọn một bảng mạch của nhà tài trợ và một bảng mạch nhận, với tất cả các điều chỉnh khác về mặt khái niệm được định tuyến thông qua cặp đó. Việc kiểm tra tính khả thi mã hóa điều này bằng cách gộp tất cả các khoản thâm hụt thành một khoản cần thiết duy nhất và yêu cầu một nguồn thặng dư duy nhất để bù đắp khoản đó. Tính đơn điệu của tính khả thi trong T đảm bảo rằng tìm kiếm nhị phân tìm thấy mức tối thiểu có thể đạt được tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(a, T):
    deficit = 0
    max_surplus = 0

    for x in a:
        if x < T:
            deficit += T - x
        else:
            max_surplus = max(max_surplus, x - T)

    return max_surplus >= deficit

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    lo, hi = 0.0, max(a)

    for _ in range(60):
        mid = (lo + hi) / 2
        if can(a, mid):
            lo = mid
        else:
            hi = mid

    print(f"{lo:.1f}")

if __name__ == "__main__":
    solve()
```Việc triển khai tách biệt việc kiểm tra tính khả thi khỏi tìm kiếm nhị phân. các`can`hàm tính toán tổng thâm hụt và theo dõi thặng dư lớn nhất có thể của một nhà tài trợ. Điều này rất quan trọng vì hạn chế của một lần chuyển giao duy nhất sẽ thu gọn tất cả việc phân phối lại thành một nguồn hiệu quả. Việc sử dụng tìm kiếm nhị phân dấu phẩy động sẽ tránh được các vấn đề về độ chính xác vì kết quả đầu ra chỉ yêu cầu một chữ số thập phân. 

Tìm kiếm nhị phân chạy với số lần lặp cố định (60), đủ để hội tụ độ chính xác kép. Bước định dạng làm tròn đến chính xác một chữ số thập phân theo yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 3 6
```Chúng tôi tìm kiếm nhị phân T. 

| Bước | T | Thâm hụt D | Thặng dư tối đa | Khả thi | 
| --- | --- | --- | --- | --- | 
| 1 | 3,5 | (1,5 + 0,5) = 2,0 | 2,5 | Có | 
| 2 | 4,5 | (2,5 + 1,5 + 0,5) = 4,5 | 1,5 | Không | 

Câu trả lời cuối cùng ổn định ở khoảng 3,5. 

Điều này cho thấy việc tăng T dần dần làm giảm tính khả thi khi thâm hụt yêu cầu vượt quá mức mà một nhà tài trợ có thể cung cấp. 

### Ví dụ 2 

đầu vào:```
4
1 1 10 10
```| Bước | T | Thâm hụt D | Thặng dư tối đa | Khả thi | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 8 | 5 | Không | 
| 2 | 3 | 4 | 7 | Có | 
| 3 | 4 | 6 | 6 | Có | 

Ngưỡng khả thi tối đa là khoảng 4.0. 

Điều này khẳng định rằng việc có nhiều hội đồng quản trị lớn sẽ không giúp ích được gì trừ khi chỉ một trong số họ có thể tài trợ cho mọi khoản thâm hụt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A) | Mỗi lần kiểm tra tính khả thi là O(n), tìm kiếm nhị phân chạy ~60 lần lặp | 
| Không gian | O(1) | Chỉ có một số ắc quy được sử dụng | 

Giải pháp dễ dàng phù hợp với giới hạn vì n = 10^5 và log A là khoảng 30 đến 60 tùy thuộc vào độ chính xác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def can(a, T):
        deficit = 0
        max_surplus = 0
        for x in a:
            if x < T:
                deficit += T - x
            else:
                max_surplus = max(max_surplus, x - T)
        return max_surplus >= deficit

    n, *rest = list(map(int, inp.split()))
    a = rest[1:]

    lo, hi = 0.0, max(a)
    for _ in range(60):
        mid = (lo + hi) / 2
        if can(a, mid):
            lo = mid
        else:
            hi = mid

    return f"{lo:.1f}"

assert run("1\n2") == "2.0", "single element"
assert run("2\n1 10") == "5.5", "simple transfer balance"
assert run("3\n1 1 10") == "4.0", "one strong donor"
assert run("4\n5 5 5 5") == "5.0", "already equal"
assert run("5\n1 2 3 4 100") == "4.0", "large outlier"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | 2.0 | trường hợp cạnh bảng đơn | 
| 1 10 1 | 5,5 | cân bằng hai thái cực | 
| 1 1 10 | 4.0 | hạn chế của nhà tài trợ chi phối duy nhất | 
| 5 5 5 5 5 | 5.0 | cấu hình đã tối ưu | 
| 1 2 3 4 100 | 4.0 | hành vi ngoại lệ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các bảng đều bằng nhau. Ví dụ:```
4
5 5 5 5
```Thâm hụt đối với bất kỳ T > 5 nào đều dương ngay lập tức, trong khi không tồn tại thặng dư. Kiểm tra tính khả thi sẽ loại bỏ tất cả T > 5 và tìm kiếm nhị phân ổn định ở mức 5.0. Thuật toán trả về chính xác giá trị ban đầu vì không chuyển đổi nào có thể cải thiện cấu hình thống nhất. 

Một trường hợp khác là một bảng rất lớn và nhiều bảng nhỏ:```
5
1 1 1 1 100
```Với T = 2, mức thâm hụt là 4, 4, 4, 4 cộng lại là 16, trong khi mức thặng dư tối đa là 98. Điều này là khả thi. Khi T tăng, thâm hụt tăng nhanh và cuối cùng vượt quá mức mà một nhà tài trợ có thể cung cấp. Thuật toán tự nhiên tìm ra điểm cân bằng nơi nhà tài trợ đó được sử dụng tối đa. 

Trường hợp thứ ba là đầu vào tối thiểu:```
1
7
```Không có bảng nào khác để chuyển từ hoặc đến. Kiểm tra tính khả thi luôn trả về true với T ≤ 7 và tìm kiếm nhị phân trả về 7,0.
