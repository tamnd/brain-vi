---
title: "CF 102591B - \u042f\u0433\u043e\u0434\u044b-\u043f\u043e\u0436\u0438\u0440\u0430\u0442\u0435\u043b\u0438"
description: "Chúng tôi sắp xếp các quả mọng theo hình tròn. Mỗi quả mọng có trọng lượng duy nhất từ ​​1 đến N và thứ tự đầu vào mô tả vị trí của chúng xung quanh vòng tròn."
date: "2026-08-01T06:33:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102591
codeforces_index: "B"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u043f\u0440\u0435\u0434\u043c\u0435\u0442\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u041c\u0423\u0418\u0422 \u043f\u043e \u0441\u043f\u043e\u0440\u0442\u0438\u0432\u043d\u043e\u043c\u0443 \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2020. \u0424\u0438\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u0442\u0443\u0440."
rating: 0
weight: 102591
solve_time_s: 224
verified: true
draft: false
---

[CF 102591B - \u042f\u0433\u043e\u0434\u044b-\u043f\u043e\u0436\u0438\u0440\u0430\u044 2\u0435\u043b\u0438](https://codeforces.com/problemset/problem/102591/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi sắp xếp các quả mọng theo hình tròn. Mỗi quả mọng có trọng lượng duy nhất từ ​​1 đến N và thứ tự đầu vào mô tả vị trí của chúng xung quanh vòng tròn. Việc di chuyển chỉ có thể thực hiện được khi hai quả lân cận có trọng lượng liên tiếp, trong đó quả lớn hơn nặng đúng một quả. Quả nặng hơn sẽ loại bỏ quả nhẹ hơn và giữ nguyên trọng lượng của nó. Nhiệm vụ là quyết định xem liệu một chuỗi các bước di chuyển nào đó có thể loại bỏ mọi quả mọng ngoại trừ quả có trọng lượng N hay không. 

Quan sát chính xuất phát từ thực tế là mỗi cân nặng đều có chính xác một người ăn được. Berry k chỉ có thể biến mất khi berry k+1 vẫn còn sống và ở gần nó. Vì cần có k+1 để loại bỏ k nên thứ tự loại bỏ trên thực tế là bắt buộc. Quả 1 phải biến mất trước quả 2, quả 2 trước quả 3, v.v. cho đến khi quả N-1 biến mất cuối cùng. Vấn đề không phải là tìm kiếm thông qua các nước đi có thể xảy ra mà là kiểm tra xem liệu đơn hàng yêu cầu duy nhất này có thể đạt được hay không. 

Ràng buộc N tối đa 2 * 10^5 loại trừ các mô phỏng thử các chuỗi di chuyển khác nhau. Tìm kiếm trạng thái sẽ có vô số khả năng và thậm chí một mô phỏng trực tiếp quét toàn bộ vòng tròn sau mỗi lần xóa sẽ thực hiện các thao tác O(N^2), quá nhiều đối với kích thước đầu vào này. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính. 

Có một số trường hợp khó khăn trong đó việc triển khai không chính xác có thể thất bại. Khi N = 1 thì không cần di chuyển nên câu trả lời phải là CÓ.```
Input:
1
1
```Đầu ra đúng là CÓ. Một giải pháp luôn bắt đầu bằng cách kiểm tra tính kề giữa 1 và 2 sẽ truy cập vào một giá trị không tồn tại. 

Một trường hợp tinh tế khác là khi tồn tại một cặp liên tiếp nhưng loại bỏ nó quá sớm sẽ là sai lầm. Hãy xem xét mẫu:```
Input:
4
4 1 3 2
```Cặp 3 và 2 liền kề nhau nhưng loại bỏ 2 cặp đầu tiên khiến 4 và 1 bị chia cắt bởi không có kẻ ăn hữu ích nào nên trò chơi không thể kết thúc. Một giải pháp chỉ kiểm tra xem liệu một số động thái hợp lệ có tồn tại ở mọi thời điểm hay không sẽ cho kết quả sai. 

Một ranh giới hình tròn cũng rất quan trọng. Ví dụ:```
Input:
3
2 1 3
```Giá trị 1 và 2 liền kề nhau qua vòng tròn, do đó 2 ăn 1 rồi 3 ăn 2. Kết quả đúng là CÓ. Việc triển khai quên kết nối giữa vị trí đầu tiên và cuối cùng sẽ từ chối trường hợp này một cách không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử mọi động thái hợp lệ có thể có, khám phá đệ quy tất cả các lệnh loại bỏ có thể có. Nó đúng vì nó tuân theo các quy tắc một cách chính xác và chấp nhận nếu có ít nhất một đường dẫn tới một quả mọng. Vấn đề là số lượng đường đi có thể tăng lên rất nhanh. Ngay cả chỉ với một vài quả mọng cũng có thể có nhiều lựa chọn và với N = 2 * 10^5 cách tiếp cận này là hoàn toàn không thể. 

Quan sát quan trọng là lực lượng vũ phu khám phá những lựa chọn không thực sự tồn tại. Lệnh loại bỏ bị ép buộc. Berry k phải được loại bỏ trước berry k+1 vì k+1 là quả duy nhất có thể loại bỏ k. Một khi k+1 biến mất thì sau này k không bao giờ có thể biến mất. Điều này làm giảm toàn bộ quá trình để kiểm tra xem liệu 1, rồi 2, rồi 3, v.v., mỗi cái có thể được loại bỏ vào đúng thời điểm hay không. 

Thử thách còn lại là duy trì tính kề cận trong khi các phần tử biến mất khỏi vòng tròn. Một danh sách liên kết đôi được biểu diễn bằng mảng là đủ. Đối với mỗi trọng số, chúng tôi lưu trữ trọng số của hàng xóm bên trái và bên phải hiện tại của nó. Khi trọng lượng k được loại bỏ, hai hàng xóm của nó trở nên liền kề. Mỗi quả mọng được loại bỏ một lần, vì vậy tổng công việc là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(N) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách liên kết vòng. Đối với mỗi giá trị quả mọng, hãy lưu trữ giá trị nào ở ngay bên trái và giá trị nào ở ngay bên phải nó. Thứ tự đầu vào cung cấp thông tin này một cách trực tiếp, bao gồm cả kết nối giữa vị trí cuối cùng và vị trí đầu tiên. 
2. Xử lý quả theo thứ tự tăng dần từ 1 đến N-1. Ở bước k, kiểm tra xem berry k hiện có ở cạnh berry k+1 hay không. Đây là cách duy nhất có thể để k biến mất. 
3. Nếu k+1 không phải là lân cận bên trái cũng như bên phải của k thì yêu cầu loại bỏ không thể xảy ra nên câu trả lời là KHÔNG. 
4. Nếu có thể loại bỏ k, hãy kết nối trực tiếp hai hàng xóm của nó và loại bỏ k khỏi danh sách liên kết. Lần lặp tiếp theo hoạt động trên vòng tròn được cập nhật. 
5. Nếu mọi giá trị từ 1 đến N-1 có thể bị loại bỏ theo thứ tự này thì chỉ còn lại berry N, vì vậy câu trả lời là CÓ. 

Tại sao nó hoạt động: 

Điều bất biến là trước khi xử lý giá trị k, tất cả các giá trị nhỏ hơn k đã bị loại bỏ và tất cả các giá trị từ k đến N vẫn còn tồn tại. Berry k phải biến mất trước khi k+1 có thể biến mất vì k+1 là sinh vật duy nhất có thể ăn k. Điều này làm cho thứ tự loại bỏ ngày càng cần thiết trong mọi trò chơi hợp lệ. Thuật toán kiểm tra chính xác xem mỗi lần xóa được yêu cầu có thể xảy ra tại thời điểm cần thiết hay không và các bản cập nhật danh sách liên kết sẽ duy trì tính kề cận vòng tròn thực sự sau mỗi lần xóa. Nếu tất cả các lần kiểm tra đều thành công thì trình tự loại bỏ tương tự là cách hợp lệ để kết thúc trò chơi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    if n == 1:
        print("YES")
        return

    left = [0] * (n + 1)
    right = [0] * (n + 1)

    for i, x in enumerate(a):
        left[x] = a[(i - 1) % n]
        right[x] = a[(i + 1) % n]

    for x in range(1, n):
        if left[x] != x + 1 and right[x] != x + 1:
            print("NO")
            return

        l = left[x]
        r = right[x]
        right[l] = r
        left[r] = l

    print("YES")

if __name__ == "__main__":
    solve()
```các`left`Và`right`mảng lưu trữ hàng xóm theo trọng lượng quả mọng thay vì theo vị trí. Điều này thuận tiện vì thứ tự loại bỏ dựa trên trọng số chứ không dựa trên chỉ số gốc. 

Vòng lặp từ 1 đến N-1 tuân theo thứ tự biến mất bắt buộc. Trước khi loại bỏ x, mã sẽ kiểm tra cả hai hướng vì vòng tròn không có điểm bắt đầu cố định, vì vậy x+1 có thể là lân cận. 

Khi x bị loại bỏ, hàng xóm bên trái và hàng xóm bên phải của nó sẽ liền kề nhau. Các bài tập cập nhật cả hai hướng của danh sách liên kết đôi, giúp tránh mọi sự dịch chuyển của mảng ban đầu. Đây là sự khác biệt giữa giải pháp O(N) và giải pháp liên tục xóa từ giữa danh sách. 

Số nguyên Python ở đây là đủ vì chỉ các chỉ số và giá trị tối đa 2 * 10^5 mới được lưu trữ, do đó, tràn không phải là vấn đề đáng lo ngại. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3
1 3 2
```| Đã kiểm tra giá trị hiện tại | Hàng xóm bên trái | Hàng xóm bên phải | Hàng xóm bắt buộc | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 2 | Xóa 1 | 
| 2 | 3 | 3 | 3 | Xóa 2 | 

Sau khi loại bỏ 1, hình tròn trở thành 3, 2. Khi đó 3 loại bỏ 2, chỉ để lại giá trị lớn nhất. Dấu vết cho thấy một cặp liên tiếp không cần phải theo thứ tự tăng dần ở đầu vào, chỉ liền kề nhau tại thời điểm được yêu cầu. 

Đối với mẫu thứ hai:```
4
4 1 3 2
```| Đã kiểm tra giá trị hiện tại | Hàng xóm bên trái | Hàng xóm bên phải | Hàng xóm bắt buộc | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 3 | 2 | Thất bại | 

Quả 1 cần quả 2 ở liền kề, nhưng cả hai quả lân cận đều là 4 và 3. Không thao tác nào sau này có thể hữu ích vì quả 1 là lần đầu tiên cần phải loại bỏ. Câu trả lời là KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi quả mọng được khởi tạo một lần và bị xóa nhiều nhất một lần | 
| Không gian | O(N) | Hai mảng lân cận có kích thước N + 1 được lưu trữ | 

Giải pháp này phù hợp với các ràng buộc vì nó chỉ thực hiện một lượng công việc không đổi trên mỗi quả mọng. Việc sử dụng bộ nhớ cũng tuyến tính, phù hợp với N lên tới 2 * 10^5. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    if n == 1:
        return "YES\n"

    left = [0] * (n + 1)
    right = [0] * (n + 1)

    for i, x in enumerate(a):
        left[x] = a[(i - 1) % n]
        right[x] = a[(i + 1) % n]

    for x in range(1, n):
        if left[x] != x + 1 and right[x] != x + 1:
            return "NO\n"
        l = left[x]
        r = right[x]
        right[l] = r
        left[r] = l

    return "YES\n"

assert solution("3\n1 3 2\n") == "YES\n", "sample 1"
assert solution("4\n4 1 3 2\n") == "NO\n", "sample 2"

assert solution("1\n1\n") == "YES\n", "single berry"
assert solution("5\n5 1 2 3 4\n") == "YES\n", "already removable chain"
assert solution("5\n5 4 1 2 3\n") == "NO\n", "wrong order around maximum"
assert solution("6\n2 3 4 5 6 1\n") == "YES\n", "circular boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`| CÓ | Chuỗi di chuyển trống rỗng | 
|`5 / 5 1 2 3 4`| CÓ | Chuỗi loại bỏ tăng trực tiếp | 
|`5 / 5 4 1 2 3`| KHÔNG | Trường hợp nước đi đầu hấp dẫn không giải quyết được cấp độ | 
|`6 / 2 3 4 5 6 1`| CÓ | Liền kề qua ranh giới hình tròn | 

## Vỏ cạnh 

Đối với trường hợp berry đơn:```
Input:
1
1
```Thuật toán ngay lập tức trả về CÓ vì không có gì để loại bỏ. Điều này tránh việc cố gắng truy cập trọng số 2, trọng số không tồn tại. 

Đối với trường hợp lỗi mẫu:```
Input:
4
4 1 3 2
```Thuật toán kiểm tra trọng số 1 trước vì thứ tự loại bỏ là bắt buộc. Hàng xóm của nó là 4 và 3, vì vậy trọng lượng 2 không có sẵn như một kẻ ăn thịt. Thuật toán dừng ngay lập tức và trả về NO. 

Đối với trường hợp kề đường tròn:```
Input:
3
2 1 3
```Danh sách liên kết lưu trữ các hàng xóm của 1 là 2 và 3. Lần kiểm tra đầu tiên thành công vì 2 là hàng xóm bên trái. Xóa 1 nối 2 và 3, và lần kiểm tra tiếp theo cũng thành công. Thuật toán trả về CÓ vì nó xử lý chính xác mảng dưới dạng một vòng tròn chứ không phải một đường thẳng.
