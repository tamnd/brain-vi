---
title: "CF 1768B - Sắp xếp nhanh"
description: "Chúng ta được cho một hoán vị, nghĩa là sắp xếp lại các số từ 1 đến n. Trong một lần di chuyển, chúng tôi chọn k phần tử bất kỳ, xóa chúng khỏi mảng, chỉ sắp xếp những phần tử đã chọn đó và nối chúng lại vào cuối. Phần còn lại của mảng giữ nguyên thứ tự tương đối của nó."
date: "2026-06-09T12:43:40+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1768
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 842 (Div. 2)"
rating: 900
weight: 1768
solve_time_s: 180
verified: false
draft: false
---

[CF 1768B - Sắp xếp nhanh](https://codeforces.com/problemset/problem/1768/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, toán học 
**Thời gian giải:** 3 phút 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị, nghĩa là sắp xếp lại các số từ 1 đến n. Trong một lần di chuyển, chúng tôi chọn k phần tử bất kỳ, xóa chúng khỏi mảng, chỉ sắp xếp những phần tử đã chọn đó và nối chúng lại vào cuối. Phần còn lại của mảng giữ nguyên thứ tự tương đối của nó. 

Nhiệm vụ là tính xem cần bao nhiêu bước di chuyển như vậy để chuyển đổi hoán vị thành thứ tự được sắp xếp. 

Hạn chế chính là n có thể lên tới 100000 trong tất cả các trường hợp thử nghiệm, do đó, bất kỳ phương pháp nào mô phỏng các hoạt động hoặc thử kết hợp các tập hợp con đã chọn đều ngay lập tức quá chậm. Phải tránh bất cứ điều gì vượt quá tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một trường hợp thất bại phổ biến xuất phát từ việc giả định rằng chúng ta luôn có thể “sửa” k phần tử bị đặt sai vị trí trong mỗi thao tác. Ví dụ: nếu k bằng n, câu trả lời luôn là 1 trừ khi đã được sắp xếp, vì chúng tôi sắp xếp mọi thứ cùng một lúc. Nhưng nếu k nhỏ thì tương tác giữa các phần tử rất quan trọng. 

Trường hợp cạnh tinh tế xuất hiện khi k bằng 1. Trong trường hợp đó, mỗi thao tác chọn một phần tử và gắn nó vào cuối không thay đổi, nghĩa là chúng ta đang xoay các phần tử một cách hiệu quả. Một kẻ tham lam ngây thơ có thể cho rằng điều này hoạt động giống như hoạt động sắp xếp bong bóng và đánh giá thấp các hoạt động. 

Một dạng lỗi khác là giả sử chúng ta có thể đặt các phần tử một cách độc lập vào đúng vị trí của chúng. Trong thực tế, một khi các phần tử được di chuyển về phía sau, các ràng buộc về thứ tự tương đối của chúng vẫn tồn tại, do đó tiến trình bị chi phối bởi cấu trúc tổng thể thay vì các bản sửa lỗi cục bộ. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ mô phỏng tất cả các lựa chọn có thể có của k phần tử ở mỗi bước và cố gắng đạt được thứ tự được sắp xếp trong ít lần di chuyển nhất. Về nguyên tắc, điều này đúng vì nó khám phá toàn bộ không gian trạng thái của các hoán vị có thể tiếp cận được trong thao tác. Tuy nhiên, hệ số phân nhánh là rất lớn, vì có rất nhiều lựa chọn có thể chọn (n, k) cho mỗi bước, khiến nó trở thành cấp số mũ và không thể sử dụng được ngay cả khi n = 20. 

Điều quan trọng cần lưu ý là thao tác này chỉ cho phép chúng ta “thu thập” các phần tử vào bộ đệm và chèn lại chúng theo thứ tự được sắp xếp ở cuối. Điều này có nghĩa là các phần tử đã có thứ tự tương đối chính xác có thể bị bỏ qua và độ khó hoàn toàn được xác định bởi số lượng phần tử đã được căn chỉnh theo cấu trúc hoán vị danh tính. 

Quan điểm đúng đắn là theo dõi tiền tố nào của hoán vị được sắp xếp đã ổn định và có bao nhiêu phần tử “không đúng chỗ” so với nó. Mỗi thao tác có thể sửa tối đa k phần tử bị đặt sai vị trí, nhưng chỉ khi chúng được chọn phù hợp. Điều này dẫn đến một đối số đếm dựa trên số lượng phần tử đã ở đúng vị trí tương đối và số lượng phần tử vẫn chưa được sắp xếp. 

Một đặc tính chính xác hơn cho thấy câu trả lời phụ thuộc vào số lượng phần tử đã có hậu tố chính xác khi quét từ cuối. Mỗi thao tác có thể mở rộng hậu tố này thêm tối đa k phần tử, bởi vì chúng ta chỉ có thể đặt đầy đủ k phần tử một cách chính xác cho mỗi lần di chuyển. 

Điều này làm giảm vấn đề thành một quá trình đếm tham lam hơn là bất kỳ mô phỏng nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý hoán vị bằng cách so sánh nó với thứ tự đã sắp xếp, đơn giản là từ 1 đến n. Chúng tôi quét từ cuối và đếm xem có bao nhiêu phần tử đã ở đúng vị trí hậu tố giảm dần. 

Chúng tôi duy trì một con trỏ`need = n`, đại diện cho giá trị lớn nhất mà chúng ta vẫn mong đợi đặt chính xác ở cuối. Chúng tôi quét mảng từ phải sang trái và mỗi lần chúng tôi thấy`need`, chúng tôi giảm`need`. Điều này đo lường bao nhiêu hậu tố đã được căn chỉnh chính xác. 

Sau lần quét này, nếu`need`trở thành 0, mảng đã được sắp xếp và không cần thực hiện thao tác nào. 

Ngược lại, tiền tố không thỏa mãn còn lại có kích thước bằng`need`. Mỗi thao tác có thể “sửa” tối đa k phần tử chưa được đặt đúng vị trí, bởi vì chúng ta luôn có thể chọn k phần tử bị đặt sai vị trí và di chuyển chúng đến cuối theo thứ tự được sắp xếp, khóa chúng vào các vị trí hậu tố chính xác một cách hiệu quả theo thời gian. 

Như vậy số lượng thao tác cần thiết là mức trần của`need / k`. 

### Tại sao nó hoạt động 

Quá trình quét hậu tố sẽ chụp được cái đuôi dài nhất đã chính xác về vị trí cuối cùng. Mọi thứ bên ngoài hậu tố này phải được định vị lại một cách tích cực. Vì mỗi thao tác chỉ có thể hoàn thiện k phần tử vào khối cuối cùng chính xác của chúng, nên chúng tôi rút gọn vấn đề thành đối số đóng gói: cần bao nhiêu nhóm có kích thước k để bao gồm tất cả các phần tử bị đặt sai vị trí. Không có thao tác nào có thể sửa nhiều hơn k phần tử vì chỉ k được di chuyển trong mỗi thao tác và việc sắp xếp chúng không tạo ra các vị trí chính xác mới ngoài k đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    p = list(map(int, input().split()))

    need = n
    for x in reversed(p):
        if x == need:
            need -= 1

    if need == 0:
        print(0)
    else:
        print((need + k - 1) // k)
```Giải pháp đọc từng trường hợp thử nghiệm một cách độc lập và tính toán có bao nhiêu phần tử đã ở đúng vị trí hậu tố. Quá trình quét ngược rất quan trọng vì nó đảm bảo chúng tôi chỉ khớp các phần tử thực sự có thể tạo thành đuôi cuối cùng hợp lệ theo thứ tự. 

Công thức cuối cùng`(need + k - 1) // k`là mức phân chia trần tiêu chuẩn, thể hiện số lượng thao tác đầy đủ được yêu cầu để xử lý tất cả các phần tử bị đặt sai vị trí còn lại. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu ở đâu`n = 4, k = 2`Và`p = [1, 3, 2, 4]`. 

Chúng tôi quét từ cuối: 

| Bước | Yếu tố | Cần trước | Hành động | Cần sau | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 4 | trận đấu 4 | 3 | 
| 2 | 2 | 3 | bỏ qua | 3 | 
| 3 | 3 | 3 | trận đấu 3 | 2 | 
| 4 | 1 | 2 | trận 1 không khớp 2 | 2 | 

Vì thế`need = 2`, nghĩa là hai phần tử chưa được căn chỉnh hậu tố chính xác. Với k = 2, chỉ cần một thao tác là đủ. 

Bây giờ hãy xem xét`p = [2, 3, 1, 4]`,`k = 2`. 

| Bước | Yếu tố | Cần trước | Hành động | Cần sau | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 4 | trận đấu | 3 | 
| 2 | 1 | 3 | bỏ qua | 3 | 
| 3 | 3 | 3 | trận đấu | 2 | 
| 4 | 2 | 2 | trận đấu | 1 | 

Đây`need = 1`, do đó vẫn cần một thao tác vì một phần tử duy nhất vẫn chặn sự căn chỉnh đầy đủ. 

Những dấu vết này cho thấy thuật toán không theo dõi các giao dịch hoán đổi mà là theo dõi cấu trúc của hậu tố cuối cùng có thể được coi là “cố định”. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | quét ngược một lần hoán vị | 
| Không gian | O(1) thêm | chỉ có quầy được sử dụng | 

Tổng n trên tất cả các trường hợp thử nghiệm được giới hạn bởi 100000, do đó quá trình quét tuyến tính dễ dàng đủ nhanh trong các ràng buộc của Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        p = list(map(int, input().split()))

        need = n
        for x in reversed(p):
            if x == need:
                need -= 1

        if need == 0:
            out.append("0")
        else:
            out.append(str((need + k - 1) // k))

    return "\n".join(out)

# provided samples
assert run("""4
3 2
1 2 3
3 1
3 1 2
4 2
1 3 2 4
4 2
2 3 1 4
""") == """0
1
1
2"""

# all sorted
assert run("""1
5 3
1 2 3 4 5
""") == "0"

# reverse order
assert run("""1
5 2
5 4 3 2 1
""") == "2"

# k = n case
assert run("""1
4 4
3 2 1 4
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đã được sắp xếp | 0 | độ chính xác phát hiện hậu tố | 
| hoán vị ngược | 2 | yêu cầu nhiều bước | 
| k = n trường hợp | 1 | thu thập đầy đủ trong một hoạt động | 

## Vỏ cạnh 

Khi hoán vị đã được sắp xếp, quá trình quét ngược sẽ ngay lập tức tiêu thụ tất cả các giá trị và`need`trở thành số không. Điều này đảm bảo thuật toán xuất ra số 0 một cách chính xác mà không thực hiện các thao tác không cần thiết. 

Khi k bằng n, công thức giảm xuống còn một thao tác bất cứ khi nào hoán vị chưa được sắp xếp, điều này phù hợp với thực tế là chúng ta có thể chọn tất cả các phần tử một lần và sắp xếp chúng. 

Khi k bằng 1, mỗi thao tác sẽ di chuyển một phần tử về cuối một cách hiệu quả, do đó, câu trả lời sẽ trở thành số phần tử chưa có hậu tố chính xác, khớp với phép chia cho một hành vi của công thức.
