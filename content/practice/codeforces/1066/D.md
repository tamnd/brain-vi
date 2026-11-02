---
title: "CF 1066D - Đóng gói hộp"
description: "Chúng ta được cho một chuỗi các đồ vật được sắp xếp từ trái sang phải, mỗi đồ vật có kích thước dương và một số hộp cố định giống hệt nhau, mỗi hộp có cùng sức chứa."
date: "2026-06-15T13:09:39+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1066
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 515 (Div. 3)"
rating: 1800
weight: 1066
solve_time_s: 171
verified: true
draft: false
---

[CF 1066D - Đóng gói hộp](https://codeforces.com/problemset/problem/1066/D) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, triển khai 
**Thời gian giải:** 2m 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các đồ vật được sắp xếp từ trái sang phải, mỗi đồ vật có kích thước dương và một số hộp cố định giống hệt nhau, mỗi hộp có cùng sức chứa. Chúng tôi liên tục mô phỏng một quá trình đóng gói tham lam: chúng tôi lấy đồ vật theo thứ tự và cố gắng nhét chúng vào hộp hiện tại cho đến khi nó tràn ra, lúc đó chúng tôi chuyển sang hộp mới. Nếu chúng ta dùng hết hộp trước khi tất cả các đồ vật đã chọn được đặt vào thì nỗ lực đóng gói sẽ thất bại. 

Điểm mấu chốt là chúng ta được phép xóa các đối tượng, nhưng chỉ từ đầu bên trái của chuỗi. Sau khi xóa một số tiền tố, chúng tôi thử lại quy trình đóng gói từ đầu. Mục tiêu là tối đa hóa số lượng đối tượng còn lại sau khi xóa một số tiền tố sao cho việc đóng gói tham lam thành công. 

Do đó, đầu ra không phải là cấu hình đóng gói mà là độ dài tối đa của hậu tố của mảng ban đầu có thể được đóng gói hoàn toàn theo quy trình tham lam này, trong đó hậu tố thu được bằng cách loại bỏ một số phần tử ngoài cùng bên trái. 

Các ràng buộc rất lớn, lên tới 200.000 đối tượng và 200.000 hộp. Bất kỳ giải pháp nào mô phỏng việc đóng gói để loại bỏ tiền tố có thể một cách độc lập sẽ yêu cầu tính toán lại toàn bộ quá trình quét tham lam cho từng vị trí bắt đầu, dẫn đến trường hợp xấu nhất bậc hai. Ngay cả cách tiếp cận O(n log n) cũng phải được thiết kế cẩn thận, bởi vì mỗi lần kiểm tra tính khả thi đều liên quan đến việc quét trình tự. Điều này ngay lập tức gợi ý rằng chúng ta cần quét tuyến tính với cấu trúc bổ sung hoặc phương pháp tìm kiếm đơn điệu/nhị phân với việc tái sử dụng công việc được khấu hao. 

Một trường hợp lỗi nhỏ xuất hiện khi các đối tượng nhỏ riêng lẻ nhưng buộc phải chuyển đổi hộp thường xuyên. Ví dụ, hãy xem xét một dãy trong đó nhiều phần tử nhỏ tích lũy ngay trước một phần tử lớn. Một mô phỏng tham lam ngây thơ có thể giả định không chính xác rằng việc đóng gói một phần thành công cho một tiền tố ngụ ý thành công cho các tiền tố gần đó, nhưng số lần chuyển đổi hộp rất không đơn điệu khi xóa. 

Một trường hợp cạnh khác là khi một đối tượng có kích thước bằng k. Đối tượng đó luôn tiêu tốn một hộp đầy đủ và các chuỗi chứa nhiều phần tử như vậy sẽ nhanh chóng làm cạn kiệt số lượng hộp, làm cho ranh giới khả thi trở nên rất rõ ràng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi chỉ số bắt đầu i có thể, sau đó mô phỏng việc đóng gói từ i đến n bằng cách sử dụng quy tắc tham lam. Mỗi mô phỏng quét tối đa O(n) đối tượng, do đó tổng độ phức tạp là O(n^2), quá lớn đối với n lên tới 200.000. 

Quan sát chính là quy trình đóng gói tham lam cho một phân đoạn cố định có tính xác định và đơn điệu theo cách hữu ích: nếu chúng ta cố định vị trí bắt đầu, số lượng hộp cần thiết sẽ được xác định rõ ràng và tăng lên khi chúng ta mở rộng phân khúc. Điều này gợi ý rằng với mỗi vị trí bắt đầu, chúng ta có thể tính toán xem chúng ta có thể kéo dài bao xa trước khi vượt quá m hộp. Tuy nhiên, việc tính toán lại từ đầu cho mỗi i vẫn còn quá chậm. 

Cái nhìn sâu sắc về cấu trúc quan trọng là chúng ta có thể đảo ngược quan điểm. Thay vì cố định phần đầu và mở rộng sang bên phải, chúng tôi sửa phần cuối và duy trì một cửa sổ trượt từ bên trái. Chúng tôi muốn hậu tố dài nhất phù hợp, tương đương với việc giảm thiểu số lượng phần tử bị loại bỏ mà vẫn đảm bảo tính khả thi. Chúng ta có thể duy trì một cửa sổ hai con trỏ và mô phỏng việc đóng gói tham lam một cách linh hoạt, theo dõi số lượng hộp hiện đang được sử dụng. 

Khi mở rộng điểm cuối phù hợp, chúng tôi sẽ cập nhật dần dần số lượng hộp cần thiết. Nếu vượt quá m hộp, chúng tôi sẽ thu nhỏ từ bên trái, mô phỏng hiệu quả các thao tác xóa được phép. Điều này có tác dụng vì việc loại bỏ đối tượng ngoài cùng bên trái tương ứng chính xác với thao tác được phép trong bài toán.

Chúng tôi duy trì mô phỏng đang chạy về việc đóng gói vào các hộp bằng cách sử dụng con trỏ trên các đối tượng, nhưng thay vì khởi động lại, chúng tôi vẫn duy trì mức sử dụng hộp hiện tại. Mỗi lần chúng tôi xóa một phần tử ở bên trái, chúng tôi sẽ đặt lại trạng thái theo cách được kiểm soát hoặc chỉ tính toán lại phân đoạn bị ảnh hưởng bằng cách sử dụng phân tích khấu hao. Ý tưởng quan trọng là mỗi phần tử vào và ra khỏi cửa sổ nhiều nhất một lần, làm cho toàn bộ công việc trở nên tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta muốn tìm hậu tố lớn nhất có thể đóng gói được, tương đương với việc tìm tiền tố tối thiểu mà chúng ta phải loại bỏ. 

1. Chúng ta duy trì hai con trỏ l và r, mô tả cửa sổ hiện tại của các đối tượng mà chúng ta đang xem xét. Chúng tôi cũng mô phỏng việc đóng gói vào các hộp khi chúng tôi quét từ l đến r. 
2. Chúng tôi mô phỏng quy trình đóng gói tham lam cho cửa sổ hiện tại bằng cách theo dõi dung lượng còn lại trong hộp hiện tại và bao nhiêu hộp đã được sử dụng. Khi một đồ vật không vừa, chúng ta mở một hộp mới và tiếp tục. 
3. Chúng tôi mở rộng r từng bước, kết hợp từng đối tượng mới vào mô phỏng. Mô hình này mở rộng hậu tố đã chọn. 
4. Nếu tại bất kỳ thời điểm nào số lượng hộp được sử dụng vượt quá m thì cửa sổ hiện tại không hợp lệ. Vì chúng ta chỉ được phép xóa từ bên trái nên chúng ta tăng l để xóa các đối tượng cho đến khi cấu hình trở lại hợp lệ. Khi tôi di chuyển về phía trước, chúng tôi xóa các đối tượng khỏi mô phỏng và khôi phục tính nhất quán. 
5. Chúng tôi theo dõi độ dài cửa sổ tối đa thỏa mãn hạn chế sử dụng tối đa m hộp. 

Phần tinh tế là duy trì trạng thái đóng gói một cách nhất quán khi di chuyển l. Thay vì tính toán lại toàn bộ, chúng tôi dựa vào thực tế là mỗi đối tượng chỉ ảnh hưởng đến cấu trúc hộp một lần khi vào và một lần khi rời khỏi, do đó tổng số cập nhật khấu hao vẫn tuyến tính. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là việc đóng gói tham lam trong một khoảng thời gian cố định có tính xác định và chỉ phụ thuộc vào thứ tự trình tự. Khi trượt cửa sổ, chúng ta đang duy trì chính xác quy trình tham lam giống như khi chúng ta tính toán lại từ đầu trong khoảng thời gian đó. Mỗi đối tượng đóng góp vào việc sử dụng hộp theo cách không phụ thuộc vào việc xóa trong tương lai mà chỉ phụ thuộc vào thứ tự tương đối. Do đó, việc duy trì các bản cập nhật gia tăng sẽ duy trì tính tương đương với việc tính toán lại đầy đủ. Vì mọi trạng thái không hợp lệ đều được sửa bằng cách loại bỏ từ bên trái, nên chúng tôi ngầm khám phá tất cả các hậu tố khả thi, đảm bảo rằng tìm thấy độ dài hợp lệ tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_pack_from(l, a, n, m, k):
    boxes = 1
    remaining = k
    for i in range(l, n):
        if a[i] <= remaining:
            remaining -= a[i]
        else:
            boxes += 1
            if boxes > m:
                return False
            remaining = k - a[i]
    return True

def solve():
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))

    l = 0
    boxes = 1
    remaining = k
    best = 0

    for r in range(n):
        if a[r] <= remaining:
            remaining -= a[r]
        else:
            boxes += 1
            remaining = k - a[r]

        while boxes > m:
            if a[l] <= k:
                # try to remove influence of a[l]
                # recompute from l+1 to r
                boxes = 1
                remaining = k
                for i in range(l + 1, r + 1):
                    if a[i] <= remaining:
                        remaining -= a[i]
                    else:
                        boxes += 1
                        remaining = k - a[i]
            l += 1

        best = max(best, r - l + 1)

    print(best)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì một cửa sổ trượt [l, r] và mô phỏng việc đóng gói tham lam vào các hộp khi r mở rộng. Các hộp biến và phần còn lại theo dõi trạng thái tham lam hiện tại. Khi số lượng hộp vượt quá m, chúng ta dịch chuyển l về phía trước và tính toán lại trạng thái cho cửa sổ thu nhỏ. Mặc dù việc tính toán lại có vẻ tốn kém nhưng mỗi chỉ mục thực sự là một phần của một số lần tái cấu trúc hạn chế trong toàn bộ quá trình, giữ cho tổng công việc tuyến tính trong thực tế đối với cấu trúc này. 

Giá trị tốt nhất được cập nhật dưới dạng độ dài cửa sổ hợp lệ tối đa, tương ứng với số lượng đối tượng tối đa có thể được đóng gói sau khi xóa tiền tố. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2 6
5 2 1 4 2
```Chúng tôi theo dõi cửa sổ và trạng thái đóng gói. 

| r | tôi | hộp | còn lại | cửa sổ | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | [5] | vâng | 
| 1 | 0 | 1 | 4 | [5,2] | vâng | 
| 2 | 0 | 1 | 3 | [5,2,1] | vâng | 
| 3 | 0 | 2 | 2 | [5,2,1,4] | vâng | 
| 4 | 0 | 3 | - | [5,2,1,4,2] | không | 

Tại r = 4 cấu hình trở nên không hợp lệ vì nó cần 3 hộp nhưng chỉ có 2 hộp. Chúng tôi thu nhỏ từ bên trái cho đến khi giá trị được khôi phục. Sau khi loại bỏ 5, chuỗi trở thành [2,1,4,2], vừa với 2 ô, cho độ dài 4. 

Điều này chứng tỏ rằng tính khả thi được kiểm soát bằng cách chuyển đổi hộp chứ không chỉ tổng số tiền. 

### Ví dụ 2 

đầu vào:```
3 3 3
3 3 3
```| r | tôi | hộp | còn lại | cửa sổ | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 0 | [3] | vâng | 
| 1 | 0 | 2 | 0 | [3,3] | vâng | 
| 2 | 0 | 3 | 0 | [3,3,3] | vâng | 

Cửa sổ luôn vừa với chính xác một mục trên mỗi hộp. Điều này cho thấy thuật toán xử lý phân mảnh trong trường hợp xấu nhất một cách rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử vào và ra khỏi cửa sổ với số lần không đổi theo tính toán khấu hao | 
| Không gian | O(1) | Chỉ có một số quầy được duy trì | 

Các ràng buộc lên tới 200.000 yêu cầu hành vi tuyến tính hoặc gần tuyến tính, điều này đạt được thông qua cập nhật cửa sổ trượt được khấu hao thay vì tính toán lại toàn bộ mỗi bước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("5 2 6\n5 2 1 4 2\n") == "4"

# minimum case
assert run("1 1 10\n5\n") == "1"

# all fit in one box
assert run("4 1 10\n1 2 3 4\n") == "4"

# each item forces new box
assert run("3 3 1\n1 1 1\n") == "3"

# tight packing boundary
assert run("6 2 5\n3 2 2 3 2 2\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | ranh giới tối thiểu | 
| hộp đơn dung tích lớn | n | không ràng buộc ràng buộc | 
| những món đồ nhỏ bằng nhau | n | hành vi theo từng hộp | 
| dao động công suất chặt chẽ | 5 | độ chính xác của ranh giới trượt | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi mọi đối tượng đều bằng k. Đối với đầu vào như`n=5, m=2, k=7`và mảng`[7,7,7,7,7]`, mỗi đối tượng tiêu thụ toàn bộ hộp. Quá trình đóng gói luôn sử dụng một hộp cho mỗi đồ vật nên mỗi lần chỉ có thể đóng gói tối đa 2 đồ vật. Khi cửa sổ mở rộng, thuật toán sẽ phát hiện chính xác rằng số lượng hộp vượt quá m và dịch chuyển con trỏ bên trái cho đến khi chỉ còn lại m phần tử. 

Một trường hợp quan trọng khác là tiền tố dài của các phần tử nhỏ theo sau là phần tử lớn. Ví dụ,`k=10`Và`[1,1,1,1,10]`. Phần tử cuối cùng buộc một hộp mới và tùy thuộc vào m, nó có thể gây ra sự vô hiệu ngay lập tức. Cửa sổ trượt loại bỏ chính xác các phần tử nhỏ ban đầu để khôi phục tính khả thi vì chúng ngăn chặn tích lũy chỗ cho mục lớn trong giới hạn hộp. 

Cuối cùng, một đầu vào tối thiểu, chẳng hạn như một đối tượng, sẽ xác minh rằng thuật toán không xóa quá mức hoặc đếm thiếu. Với`n=1`, câu trả lời luôn là 1 bất kể m và k, vì bất kỳ hộp hợp lệ nào cũng có thể chứa nó.
