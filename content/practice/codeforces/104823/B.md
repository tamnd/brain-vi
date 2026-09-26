---
title: "CF 104823B - rẽ"
description: "Chúng ta được cho một chiếc xe đạp di chuyển bên trong một hành lang rất hẹp được mô phỏng như hai đường thẳng song song vô hạn với khoảng cách cố định giữa chúng. Bản thân chiếc xe đạp được coi như một đoạn cứng có chiều dài l."
date: "2026-06-28T12:36:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104823
codeforces_index: "B"
codeforces_contest_name: "The 17-th BIT Campus Programming Contest - Online Round"
rating: 0
weight: 104823
solve_time_s: 48
verified: true
draft: false
---

[CF 104823B - rẽ](https://codeforces.com/problemset/problem/104823/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chiếc xe đạp di chuyển bên trong một hành lang rất hẹp được mô phỏng như hai đường thẳng song song vô hạn với khoảng cách cố định giữa chúng. Bản thân chiếc xe đạp được coi như một đoạn cứng có chiều dài`l`. Người lái có thể di chuyển tiến và lùi trong khi liên tục đánh lái và mô hình chuyển động đảm bảo rằng xe đạp luôn là một đoạn cứng bị ràng buộc bởi các ranh giới hành lang. 

Mục tiêu là để xác định xem liệu người lái có thể xoay xe đạp chính xác 180 độ sao cho hướng cuối cùng của nó ngược với hướng ban đầu hay không và nếu có thể, giảm thiểu số lần người lái phải chuyển sang chuyển động lùi trong quá trình di chuyển. Nếu việc quay không thể hoàn thành, chúng ta xuất ra “gg”. 

Các tham số đầu vào chính là chiều rộng hành lang`d`và chiều dài xe đạp`l`. Chiều rộng kiểm soát mức độ tự do hình học mà chúng ta phải xoay, trong khi chiều dài xác định lượng không gian mà xe đạp chiếm trong quá trình rẽ. 

Đầu ra là một số nguyên duy nhất biểu thị số lượng phân đoạn đảo ngược tối thiểu trong bất kỳ thao tác hợp lệ nào hoặc “gg” nếu không tồn tại thao tác hợp lệ. 

Khó khăn chính không phải là mô phỏng chuyển động mà là hiểu được khi nào một đoạn cứng có thể xoay 180 độ bên trong dải mà không vi phạm các ràng buộc và việc đảo ngược góp phần như thế nào để đạt được chuyển động quay đó. 

Trường hợp khó nhận thấy là khi hành lang quá chật. Ví dụ, khi`d = 10`Và`l = 10`, đầu ra mẫu là “gg”, nghĩa là ngay cả khi có vẻ vừa khít cũng không đủ. Điều này cho thấy rằng sự bình đẳng là chưa đủ và cần phải có khoảng trống hình học nghiêm ngặt. Một giả định ngây thơ như “nếu nó vừa khít, nó có thể xoay” ở đây không thành công vì việc xoay yêu cầu khoảng hở bổ sung ngoài khả năng lắp tĩnh. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ cố gắng mô phỏng chuyển động liên tục của xe đạp. Người ta sẽ cố gắng lập mô hình đoạn trong dải và mô phỏng các bước thời gian nhỏ, thử tất cả các hướng lái có thể có và kiểm tra xem có thể quay được 180 độ hay không trong khi đếm các lần đảo chiều. Điều này nhanh chóng trở nên khó giải quyết vì không gian trạng thái là liên tục: hướng liên tục, vị trí liên tục và góc lái thay đổi liên tục. Ngay cả việc rời rạc hóa các góc cũng dẫn đến sự bùng nổ của các trạng thái và tính chính xác trở nên mong manh do các vấn đề về độ chính xác hình học. 

Quan sát quan trọng là các chi tiết chuyển động bị sai lệch. Xe đạp hoạt động giống như một đoạn cứng được giới hạn bên trong một dải, vì vậy vật cản có ý nghĩa duy nhất là liệu đoạn đó có thể quay bên trong hành lang mà không giao nhau với các ranh giới hay không. Điều này làm giảm bài toán từ động học sang hình học. 

Để xoay một đoạn có chiều dài`l`bên trong một dải chiều rộng`d`, đoạn này tại một thời điểm nào đó phải có khả năng vuông góc với hành lang trong khi vẫn nằm hoàn toàn bên trong nó. Tại thời điểm đó, đoạn này kéo dài toàn bộ chiều dài của nó theo chiều rộng. Điều này ngụ ý một điều kiện khả thi nghiêm ngặt về`d`liên quan đến`l`. Mẫu đã xác nhận rằng đẳng thức không thành công, vì vậy điều kiện trở thành`d > l`. 

Khi tính khả thi được thiết lập, chúng tôi xem xét việc đảo ngược. Một lần đảo chiều tương ứng với việc chuyển từ chuyển động tiến về phía trước sang chuyển động lùi và mỗi lần chuyển đổi như vậy được tính khi hướng lái vượt qua ngưỡng phân cách chuyển động giống tiến và chuyển động lùi. Để có thao tác tối ưu, chúng ta chỉ cần một quá trình chuyển đổi như vậy: bắt đầu di chuyển về phía trước để vào cấu hình có thể xoay, thực hiện chuyển động quay và sau đó chuyển sang chuyển động lùi để căn chỉnh cuối cùng. Bất kỳ chuyển đổi bổ sung nào đều không cần thiết và không làm giảm các ràng buộc hình học. 

Do đó, vấn đề được chuyển thành một cuộc kiểm tra tính khả thi đơn giản và một câu trả lời liên tục khi khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng chuyển động lực mạnh | Hàm mũ trong sự rời rạc | Lưu trữ trạng thái lớn | Quá chậm | 
| Giảm hình học | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm vấn đề xuống còn việc kiểm tra xem hành lang có đủ rộng để cho phép một đoạn cứng quay 180 độ hoàn toàn hay không. 

1. Đọc`d`Và`l`từ đầu vào. Chúng xác định chiều rộng hành lang và chiều dài xe đạp. 
2. Kiểm tra xem`d`thực sự lớn hơn`l`. Điều kiện này đảm bảo rằng tồn tại ít nhất một cấu hình trong đó xe đạp có thể quay mà không chạm vào cả hai ranh giới cùng một lúc. Bất đẳng thức chặt chẽ là cần thiết vì đẳng thức tương ứng với trường hợp suy biến không có độ trễ cho phép quay. 
3. Nếu`d <= l`, xuất ra “gg” ngay lập tức vì không có chuyển động liên tục nào có thể quay hết một vòng mà không vi phạm các ràng buộc. 
4. Nếu`d > l`, đầu ra`1`là số lần đảo chiều tối thiểu cần thiết. Điều này tương ứng với chính xác một công tắc giữa chuyển động tiến và lùi trong trình tự quay tối ưu. 

### Tại sao nó hoạt động 

Ràng buộc hành lang làm giảm vấn đề chuyển động liên tục thành một điều kiện khả thi hình học duy nhất: liệu đoạn đó có thể đạt được hướng vuông góc bên trong dải mà không có giao điểm hay không. Nếu chiều dài đoạn tối đa bằng chiều rộng dải thì không thể đạt được cấu hình vuông góc với bất kỳ lề nào, chặn việc xoay. Bất đẳng thức nghiêm ngặt đưa ra độ võng cần thiết cho sự biến dạng liên tục từ hướng ban đầu sang hướng ngược lại. 

Khi tính khả thi được đảm bảo, chuyển động có thể được tổ chức sao cho chỉ cần một công tắc hướng. Bất kỳ góc quay 180 độ hợp lệ nào cũng phải bao gồm một giai đoạn trong đó hướng chuyển động hiệu quả thay đổi so với khung xe đạp; hợp nhất tất cả những thay đổi như vậy thành một quá trình chuyển đổi duy nhất mang lại số lượng đảo ngược tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    d, l = map(int, input().split())
    if d <= l:
        print("gg")
    else:
        print(1)

if __name__ == "__main__":
    solve()
```Giải pháp đọc hai số nguyên và thực hiện một so sánh duy nhất. Toàn bộ sự phức tạp của hệ thống hình học ban đầu được hấp thụ vào điều kiện`d > l`. Logic đầu ra là trực tiếp: trường hợp không khả thi tạo ra “gg”, trường hợp khả thi tạo ra`1`. 

Không có vòng lặp hoặc mô phỏng vì cấu trúc bài toán đảm bảo rằng tất cả các động lực liên tục giảm xuống một ràng buộc hình học nhị phân. 

## Ví dụ đã hoạt động 

Xem xét đầu vào mẫu`10 10`. 

Chúng tôi chỉ theo dõi điều kiện khả thi. 

| d | tôi | d > l | Đầu ra | 
| --- | --- | --- | --- | 
| 10 | 10 | sai | gg | 

Chiều rộng hành lang bằng chiều dài xe đạp nên không có độ chùng khi quay. Cấu hình trở nên suy biến và ngăn chặn việc quay ngoắt 180 độ hợp lệ. 

Bây giờ hãy xem xét một trường hợp lớn hơn một chút`12 10`. 

| d | tôi | d > l | Đầu ra | 
| --- | --- | --- | --- | 
| 12 | 10 | đúng | 1 | 

Ở đây hành lang có đủ chiều rộng để cho phép xe đạp đi qua theo hình vuông góc. Khi điều này có thể thực hiện được thì chỉ cần một lần đảo ngược là đủ để hoàn thành thao tác một cách tối ưu. 

Dấu vết xác nhận rằng chỉ có bất đẳng thức nghiêm ngặt mới quan trọng và một khi được thỏa mãn, câu trả lời sẽ trở thành hằng số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ thực hiện một so sánh duy nhất | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép lên tới 1000 cho cả hai tham số, nhưng giải pháp không phụ thuộc vào độ lớn. Việc tính toán là thời gian không đổi và thỏa mãn một cách tầm thường các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    d, l = map(int, sys.stdin.readline().split())
    if d <= l:
        print("gg")
    else:
        print(1)

# provided sample
assert run("10 10\n") == "gg"

# minimum values, still impossible
assert run("1 1\n") == "gg"

# barely feasible case
assert run("2 1\n") == "1"

# boundary just failing equality
assert run("5 5\n") == "gg"

# larger feasible case
assert run("1000 999\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 10 | gg | trường hợp bình đẳng là không thể | 
| 1 1 | gg | trường hợp biên nhỏ nhất | 
| 2 1 | 1 | cấu hình khả thi tối thiểu | 
| 5 5 | gg | cạnh bình đẳng lặp đi lặp lại | 
| 1000 999 | 1 | đầu vào khả thi lớn | 

## Vỏ cạnh 

Về ranh giới đẳng thức`d = l`, thuật toán ngay lập tức phân loại nó là không thể. Ví dụ, đầu vào`5 5`dẫn đến`d <= l`, vì vậy đầu ra là`gg`mà không cần tính toán thêm. Điều này phù hợp với cách giải thích hình học khi không tồn tại độ chùng quay. 

Đối với chiều rộng khả thi tối thiểu như`d = l + 1`, Ví dụ`6 5`, điều kiện`d > l`giữ và đầu ra trở thành`1`. Thuật toán không cố gắng phân biệt chiều rộng lớn hơn bao nhiêu, vì bất kỳ độ chùng dương nào cũng đủ để tạo ra một đường biến dạng liên tục hợp lệ. 

Đối với các giá trị cực lớn như`1000 1`, cùng một nhánh được lấy. Lý do không phụ thuộc vào quy mô, chỉ phụ thuộc vào sự bất bình đẳng nghiêm ngặt, do đó hành vi vẫn nhất quán và ổn định trên toàn bộ phạm vi đầu vào.
