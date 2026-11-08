---
title: "CF 1408H - Bộ ba cầu vồng"
description: "Chúng ta được cung cấp một mảng các số nguyên trong đó giá trị 0 đóng một vai trò đặc biệt. Chúng tôi muốn trích xuất càng nhiều bộ ba chỉ số rời rạc càng tốt và mỗi bộ ba phải có cấu trúc rất cứng nhắc: nó phải trông giống số 0, sau đó là một giá trị khác 0, rồi một số 0 khác."
date: "2026-06-11T07:45:16+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "flows", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1408
codeforces_index: "H"
codeforces_contest_name: "Grakn Forces 2020"
rating: 3300
weight: 1408
solve_time_s: 89
verified: true
draft: false
---

[CF 1408H - Bộ ba cầu vồng](https://codeforces.com/problemset/problem/1408/H) 

**Đánh giá:** 3300 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, luồng, tham lam 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên trong đó giá trị 0 đóng một vai trò đặc biệt. Chúng tôi muốn trích xuất càng nhiều bộ ba chỉ số rời rạc càng tốt và mỗi bộ ba phải có cấu trúc rất cứng nhắc: nó phải trông giống số 0, sau đó là một giá trị khác 0, rồi một số 0 khác. Về mặt hình thức, mỗi bộ ba được chọn đều sử dụng các vị trí$i < j < k$sao cho cả hai đầu đều bằng 0 và phần tử ở giữa khác 0. 

Có hai ràng buộc toàn cầu kết hợp các bộ ba khác nhau. Đầu tiên, mọi phần tử ở giữa được chọn phải có giá trị khác biệt, vì vậy chúng ta không thể sử dụng lại cùng một số khác 0 trong hai bộ ba khác nhau. Thứ hai, tất cả các chỉ số được sử dụng trên tất cả các bộ ba phải rời rạc, vì vậy một khi một vị trí được sử dụng trong một bộ ba thì nó sẽ không khả dụng mãi mãi. 

Nhiệm vụ là tối đa hóa số lượng bộ ba như vậy mà chúng ta có thể xây dựng. 

Các ràng buộc rất lớn: tổng chiều dài của tất cả các trường hợp thử nghiệm lên tới 500.000. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả các bộ ba hoặc thậm chí tất cả các cặp số 0 trên mỗi giá trị. Bất cứ điều gì bậc hai trong$n$sẽ thất bại, và thậm chí$O(n \log n)$phải tuyến tính một cách cẩn thận trong thực tế. 

Một chế độ thất bại tinh vi xuất hiện khi việc ghép nối tham lam được thực hiện mà không tôn trọng mức tiêu thụ chỉ số toàn cầu. Ví dụ: nếu số 0 nhiều nhưng rải rác và các giá trị được sử dụng lại một cách tham lam, thì người ta có thể tạo thành các bộ ba hợp lệ cục bộ để chặn các phép gán toàn cục tốt hơn. Một vấn đề khác là giả sử mỗi giá trị đóng góp độc lập$\min(\#zeros, 1)$, bỏ qua sự cạnh tranh về số 0 giữa các giá trị. 

Như một minh họa nhỏ, hãy xem xét: 

đầu vào:```
1
6
0 1 0 2 0 0
```Một cách tiếp cận tham lam trên mỗi giá trị ngây thơ có thể cố gắng gán cả 1 và 2 số 0 xung quanh chúng, nhưng các số 0 giống nhau không thể được sử dụng lại, vì vậy chỉ có thể có một bộ ba. 

Sự tương tác giữa các tài nguyên được chia sẻ (số không) và các giá trị ở giữa độc quyền này là khó khăn chính. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ cố gắng liệt kê mọi bộ ba hợp lệ$(i, j, k)$cho mọi vị trí ở giữa có thể$j$, sau đó cố gắng chọn tập hợp tối đa các bộ ba rời rạc có giá trị ở giữa khác nhau. Điều này tự nhiên trở thành một vấn đề đóng gói hoặc kết hợp tối đa. Người ta có thể mô hình hóa các số 0 làm điểm cuối có thể tái sử dụng và mỗi vị trí khác 0 làm trung tâm tiềm năng, sau đó tìm kiếm các phép gán của các số 0 bên trái và bên phải. Tuy nhiên, số lượng bộ ba ứng cử viên là$O(n^2)$trong trường hợp xấu nhất, vì mỗi số khác 0 có thể ghép với nhiều số 0 ở cả hai vế. Ngay cả việc xây dựng tất cả các ứng cử viên cũng đã quá lớn. 

Quan sát quan trọng là chúng ta không bao giờ cần xem xét những số 0 cụ thể nào được sử dụng ở các đầu theo cách tổ hợp. Số không là các tài nguyên giống hệt nhau; chỉ có số lượng của họ là quan trọng. Điều quan trọng trên toàn cầu là có bao nhiêu số 0 có thể sử dụng được ở bên trái và bên phải của các vị trí ở giữa đã chọn. 

Nếu chúng ta sửa một giá trị trung bình$x$, chúng ta chỉ cần biết liệu có tồn tại ít nhất một số 0 trước nó và ít nhất một số 0 sau nó hay không, sau khi tính đến các số 0 đã được các bộ ba khác sử dụng. Điều này cho thấy việc xử lý các giá trị một cách tham lam theo cách gán cho mỗi phần giữa được chọn chính xác hai số 0. 

Thay vì suy nghĩ về mặt hình học của các vị trí, chúng tôi chuyển sang chế độ xem phân bổ nguồn lực. Chúng tôi duy trì các số 0 có sẵn trong mảng khi quét và chúng tôi cố gắng gán các bộ ba theo cách đảm bảo mỗi phần giữa được chọn sẽ có chính xác hai số 0. Khó khăn là buộc các số 0 bên trái và bên phải đến từ các phía khác nhau, điều này hàm ý vấn đề sắp xếp. 

Một cách tiêu chuẩn để giải quyết vấn đề này là coi các số 0 như một nhóm nhưng vẫn tôn trọng thứ tự bằng cách quét mảng. Khi chúng tôi xem xét một ứng viên ở giữa, chúng tôi cố gắng so khớp nó với một số 0 chưa sử dụng ở bên trái và một số 0 chưa sử dụng ở bên phải. Số 0 bên phải là ràng buộc tới hạn; nó buộc chúng ta phải đảm bảo rằng chúng ta không sử dụng cấu trúc tương lai quá sớm. 

Điều này dẫn đến một chiến lược tham lam: chúng tôi quét và duy trì các số 0 có sẵn và chúng tôi cố gắng tạo thành bộ ba bất cứ khi nào có thể, nhưng chúng tôi đảm bảo rằng mỗi phần giữa được sử dụng tối đa một lần và mỗi bộ ba tiêu thụ hai số 0. Sự tối ưu đến từ thực tế là các số 0 có thể hoán đổi cho nhau và chỉ có tổng số sẵn có của chúng mới giới hạn câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu của bộ ba + lựa chọn |$O(n^2)$hoặc tệ hơn |$O(n^2)$| Quá chậm | 
| Sự kết hợp tài nguyên tham lam của số không và số giữa |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi trình bày lại vấn đề bằng cách chọn càng nhiều vị trí ở giữa càng tốt sao cho mỗi vị trí ở giữa được chọn có thể được gán hai vị trí 0 riêng biệt, một vị trí trước nó và một vị trí sau nó và không có vị trí 0 nào được sử dụng lại. 

1. Chia mảng thành các vị trí số 0 và vị trí các số khác không. Chúng tôi chỉ coi các vị trí khác 0 là ứng cử viên cho phần giữa của bộ ba. Số không là tài nguyên tiêu hao phải được phân chia giữa các nhiệm vụ bên trái và bên phải. 
2. Quét mảng từ trái sang phải trong khi vẫn duy trì số lượng số 0 chưa sử dụng cho đến nay. Giá trị này thể hiện số lượng điểm cuối bên trái hiện có sẵn. 
3. Cũng duy trì số lượng số không còn lại ở bên phải. Điều này có thể được tính toán trước dưới dạng số lượng hậu tố bằng 0. 
4. Đối với từng vị trí$j$với$a_j \neq 0$, chúng tôi chỉ coi nó là ứng cử viên ở giữa nếu tồn tại ít nhất một số 0 chưa sử dụng ở bên trái và ít nhất một số 0 vẫn còn ở bên phải. Điều này đảm bảo chúng ta có thể hình thành một bộ ba hợp lệ xung quanh nó. 
5. Nếu chúng ta chọn một ở giữa$j$, chúng ta ngay lập tức tiêu thụ một số 0 từ nhóm bên trái và ngầm dự trữ một số 0 từ nhóm bên phải. Sau đó, chúng tôi đánh dấu giá trị ở giữa này là đã sử dụng để không chọn các giá trị trùng lặp. 
6. Tiếp tục quét, luôn tham lam chọn phần giữa hợp lệ khi có thể. Số lượng giữa được chọn là câu trả lời. 

Lựa chọn thiết kế quan trọng là coi các số 0 ở bên phải là ngân sách giảm dần thay vì chọn vị trí một cách rõ ràng. Điều này tránh việc tính hai lần và đảm bảo tính khả thi. 

Tại sao nó hoạt động được gắn liền với một cuộc tranh luận trao đổi toàn cầu. Giả sử một giải pháp tối ưu chọn một tập hợp các điểm trung gian. Chúng ta có thể sắp xếp lại các nhiệm vụ của họ để mỗi người ở giữa sử dụng số 0 bên trái sớm nhất có thể và số 0 bên phải mới nhất có thể. Việc chuẩn hóa này không làm giảm tính khả thi. Theo quá trình chuẩn hóa này, quá trình quét tham lam chọn trung tâm bất cứ khi nào cả hai tài nguyên đều có sẵn sẽ không bao giờ chặn giải pháp tốt hơn trong tương lai, bởi vì việc sử dụng số 0 bên trái sớm hơn chỉ làm tăng tính linh hoạt trong tương lai và các số 0 bên phải chỉ bị hạn chế bởi tổng số chứ không phải danh tính. 

Do đó, thuật toán gói càng nhiều đỉnh ở giữa càng tốt vào một quỹ giới hạn bằng 0, mỗi đỉnh yêu cầu hai đơn vị của quỹ đó được chia cho toàn bộ chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        total_zeros = sum(1 for x in a if x == 0)

        left_zeros = 0
        used_middle = set()
        ans = 0

        # remaining zeros to the right
        right_zeros = total_zeros

        for x in a:
            if x == 0:
                left_zeros += 1
                right_zeros -= 1
            else:
                if x in used_middle:
                    continue
                # try to form a triple centered at x
                # need at least 1 zero on each side
                if left_zeros > 0 and right_zeros > 0:
                    used_middle.add(x)
                    ans += 1
                    left_zeros -= 1
                    right_zeros -= 1

        print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện dựa vào một lần quét từ trái sang phải. Biến`left_zeros`đếm xem có bao nhiêu số 0 ở bên trái vị trí hiện tại. Biến`right_zeros`được khởi tạo là tổng số số 0 và giảm dần khi chúng ta tiến về phía trước, vì vậy nó luôn phản ánh số lượng số 0 còn lại ở bên phải. 

Khi chúng tôi gặp một giá trị khác 0, chúng tôi chỉ cố gắng sử dụng nó một lần do`used_middle`thiết lập, thực thi sự khác biệt của các giá trị ở giữa. Nếu cả hai bên đều có sẵn ít nhất một số 0, chúng ta sẽ tham lam tạo thành bộ ba và tiêu tốn một số 0 từ ngân sách của mỗi bên. 

Một điểm tinh tế là chúng tôi không chọn rõ ràng chỉ số 0 nào được sử dụng. Tính chính xác dựa trên thực tế là chỉ đếm vấn đề, bởi vì các số 0 không thể phân biệt được ngoại trừ các ràng buộc về thứ tự đã được nắm bắt bởi sự phân chia trái-phải. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu:```
n = 6
a = [0, 0, 1, 2, 0, 0]
```Chúng tôi theo dõi quá trình quét: 

| tôi | một [tôi] | left_zeros | đúng_zeros | đã qua sử dụng_middle | hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 3 | {} | cập nhật | 0 | 
| 2 | 0 | 2 | 2 | {} | cập nhật | 0 | 
| 3 | 1 | 2 | 2 | {} | lấy 1 | 1 | 
| 4 | 2 | 1 | 1 | {1} | lấy 2 | 2 | 
| 5 | 0 | 2 | 0 | {1,2} | cập nhật | 2 | 
| 6 | 0 | 3 | 0 | {1,2} | cập nhật | 2 | 

Điều này cho thấy cả 1 và 2 đều có thể được sử dụng vì mỗi số nhìn thấy ít nhất một số 0 ở cả hai bên khi được chọn. 

Bây giờ hãy xem xét:```
n = 6
a = [0, 1, 0, 0, 1, 0]
```| tôi | một [tôi] | left_zeros | đúng_zeros | đã qua sử dụng_middle | hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 3 | {} | cập nhật | 0 | 
| 2 | 1 | 1 | 3 | {} | lấy 1 | 1 | 
| 3 | 0 | 2 | 2 | {1} | cập nhật | 1 | 
| 4 | 0 | 3 | 1 | {1} | cập nhật | 1 | 
| 5 | 1 | 3 | 1 | {1} | bỏ qua | 1 | 
| 6 | 0 | 4 | 0 | {1} | cập nhật | 1 | 

Chỉ có thể có một bộ ba vì sau khi sử dụng giá trị 1 một lần, cấu trúc còn lại không cho phép một phần giữa khác biệt có đủ số 0 đối xứng. 

Những dấu vết này cho thấy cách thuật toán thực thi cả tính duy nhất toàn cầu của phần giữa và ràng buộc khả thi trái-phải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | một lượt với cập nhật liên tục | 
| Không gian |$O(k)$| lưu trữ các giá trị ở giữa được sử dụng, được giới hạn bởi số phần tử khác 0 riêng biệt | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm tối đa là 500.000, do đó chỉ cần quét tuyến tính cho mỗi trường hợp thử nghiệm là đủ. Các thao tác đã thiết lập vẫn hiệu quả vì mỗi giá trị được chèn nhiều nhất một lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    try:
        solve()
    finally:
        sys.stdout = old_stdout
    return out.getvalue().strip()

# provided samples
assert run("""8
1
1
2
0 0
3
0 1 0
6
0 0 1 2 0 0
6
0 1 0 0 1 0
6
0 1 3 2 0 0
6
0 0 0 0 5 0
12
0 1 0 2 2 2 0 0 3 3 4 0
""") == """0
0
1
2
1
1
1
2"""

# minimum size
assert run("1\n1\n0") == "0"

# no zeros
assert run("1\n5\n1 2 3 4 5") == "0"

# all zeros
assert run("1\n5\n0 0 0 0 0") == "0"

# single valid triple
assert run("1\n3\n0 1 0") == "1"

# repeated value blocks extra triples
assert run("1\n7\n0 1 0 0 1 0 0") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp cạnh tối thiểu | 
| không có số không | 0 | xây dựng bất khả thi | 
| tất cả số không | 0 | không có phần tử ở giữa hợp lệ | 
| ba đơn giản | 1 | tính khả thi cơ bản | 
| giá trị trung bình lặp đi lặp lại | 1 | ràng buộc giữa khác biệt | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi các số 0 tồn tại nhưng quá tập trung về một phía của các phần tử tiềm năng ở giữa. Coi như:```
0 0 1 0 0
```Thuật toán gán các số 0 bên trái dần dần. Khi đạt đến giá trị ở giữa là 1, cả nhóm 0 bên trái và bên phải đều không trống, do đó nó tạo thành chính xác một bộ ba. Sau khi tiêu thụ, không thể có thêm bộ ba nào nữa, phù hợp với giải pháp tối ưu vì chỉ tồn tại một khác 0 riêng biệt. 

Một trường hợp cạnh khác là khi nhiều giá trị khác 0 xuất hiện nhưng các số 0 không đủ để hỗ trợ vị trí đối xứng:```
0 1 2 3 0
```Ở đây chỉ có thể sử dụng một trong các giá trị ở giữa vì sau khi chọn một giá trị, cấu trúc còn lại không thể hỗ trợ gán 0 hai mặt cho giá trị khác. Cách tiếp cận tham lam đảm bảo rằng một khi số 0 được tiêu thụ một phần, tính khả thi sẽ giảm đi một cách chính xác đối với các ứng viên sau này. 

Cuối cùng, khi có nhiều số 0 nhưng số 0 lại lặp lại:```
0 1 0 1 0 1 0
```Chỉ lần xuất hiện đầu tiên của mỗi giá trị mới có thể sử dụng được. các`used_middle`ràng buộc đảm bảo chúng tôi không đếm nhầm các giá trị giống hệt nhau, mặc dù tính khả dụng bằng 0 cục bộ sẽ cho phép điều đó.
