---
title: "CF 102791E - Số trên bảng trắng"
description: "Bảng bắt đầu với mọi số nguyên từ 1 đến n được viết đúng một lần. Một phép toán sẽ loại bỏ hai số hiện có a và b và thay thế chúng bằng mức trung bình tối đa của chúng. Sau khi lặp lại đúng n - 1 lần thì chỉ còn lại một số."
date: "2026-07-27T18:11:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102791
codeforces_index: "E"
codeforces_contest_name: "ICPC 2020-2021 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102791
solve_time_s: 78
verified: true
draft: false
---

[CF 102791E - Các số trên bảng trắng](https://codeforces.com/problemset/problem/102791/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bảng bắt đầu với mọi số nguyên từ 1 đến n được viết đúng một lần. Một phép toán sẽ loại bỏ hai số hiện có a và b và thay thế chúng bằng mức trung bình tối đa của chúng. Sau khi lặp lại đúng n - 1 lần thì chỉ còn lại một số. Nhiệm vụ là làm cho số cuối cùng đó càng nhỏ càng tốt và in ra chuỗi các cặp đã chọn đạt được số đó. 

Giá trị của n có thể lớn tới 200000, do đó, bất kỳ phương pháp nào liên tục tìm kiếm qua tất cả các giá trị bảng hiện tại hoặc mô phỏng nhiều lựa chọn có thể xảy ra đều không thể thực hiện được. Một giải pháp chỉ cần thực hiện một lượng công việc không đổi cho mỗi thao tác, vì đã có n - 1 thao tác phải được in. Điều này loại trừ các cách tiếp cận có hành vi bậc hai chẳng hạn như thử mọi cặp có thể. 

Một sai lầm phổ biến là cho rằng luôn kết hợp hai giá trị nhỏ nhất là tối ưu. Hoạt động này không phải là tính trung bình thông thường vì kết quả được làm tròn lên và việc xây dựng cần kiểm soát những giá trị nào tồn tại chứ không chỉ giảm mức tối thiểu hiện tại. Ví dụ: khi n = 3, đầu vào là:```
3
```Một chiến lược tham lam bất cẩn có thể kết hợp 1 và 2 trước, tạo ra 2, sau đó kết hợp 2 và 3, tạo ra 3. Giá trị cuối cùng trở thành 3, nhưng đầu ra tối ưu để lại 2. Trình tự đúng là kết hợp 2 và 3 trước, tạo ra 3, sau đó kết hợp 1 và 3, tạo ra 2. 

Một trường hợp cạnh khác là n = 2. Không có chỗ để tạo các giá trị trùng lặp hoặc thực hiện việc xây dựng nhiều bước. Hoạt động duy nhất có thể thực hiện được là kết hợp 1 và 2, cho kết quả là 2, do đó, bất kỳ thuật toán nào giả định rằng nó luôn có thể sử dụng giá trị lặp lại sẽ thất bại. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ thử các cặp khác nhau để tìm ra số cuối cùng nhỏ nhất có thể. Điều này đúng vì mọi chuỗi thao tác hợp lệ đều có thể được biểu diễn dưới dạng cây gồm các cặp lựa chọn và việc khám phá tất cả các cây cuối cùng sẽ tìm ra kết quả tối ưu. Vấn đề là số lượng các lựa chọn có thể tăng lên cực kỳ nhanh chóng. Ngay cả thao tác đầu tiên cũng có khoảng n2 cặp có thể và có n - 1 thao tác, khiến việc khám phá toàn diện vượt xa giới hạn sẵn có. 

Quan sát quan trọng là câu trả lời tối thiểu có thể luôn là 2. Phép toán không bao giờ có thể tạo ra giá trị nhỏ hơn 2 ở cuối vì phép toán cuối cùng kết hợp hai số nguyên dương và cặp số nhỏ nhất có thể có sau khi rút gọn không thể tạo ra 1. Thử thách còn lại là xây dựng các phép toán đạt tới 2. 

Việc xây dựng hoạt động bằng cách liên tục tạo ra một giá trị lớn được kiểm soát và di chuyển nó xuống dưới. Đầu tiên, gộp n và n - 2. Kết quả là n - 1 vì trung bình cộng của hai số này đúng bằng n - 1. Sau đó gộp n - 1 với chính nó, cho ra n - 1 lần nữa. Sau đó, ý tưởng tương tự có thể được áp dụng cho các số thấp hơn còn lại: kết hợp i với i + 2 để giảm i. Mỗi thao tác sẽ loại bỏ một số không sử dụng trong khi vẫn giữ nguyên giá trị quan trọng. Cuối cùng số duy nhất còn lại là 2. 

Brute-force hoạt động vì nó khám phá mọi mức giảm có thể, nhưng nó thất bại khi n tăng lên vì không gian tìm kiếm rất lớn. Nhận xét rằng thao tác có thể bảo toàn các giá trị được lựa chọn cẩn thận cho phép chúng ta thay thế việc tìm kiếm bằng cách xây dựng trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số cây có thể thực hiện) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) ngoài đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. In 2 làm giá trị cuối cùng tối thiểu. Công trình dưới đây luôn để lại đúng con số này. 
2. Nếu n bằng 2, kết hợp trực tiếp 1 và 2. Điểm trung bình làm tròn của họ là 2, đây là câu trả lời duy nhất có thể. 
3. Đối với n lớn hơn 2, kết hợp n và n - 2. Giá trị được tạo ra là n - 1 vì ceil((n + n - 2) / 2) bằng n - 1. 
4. Kết hợp n - 1 với n - 1. Điều này giữ cho n - 1 không thay đổi và loại bỏ giá trị n - 1 ban đầu. 
5. Với mọi i từ n - 3 xuống 1, kết hợp i và i + 2. Bảng hiện tại chứa i + 2 ở giai đoạn này và thao tác tạo ra i + 1, trở thành giá trị cần thiết cho bước tiếp theo. 

Lý do công trình giảm xuống một đơn vị là vì mọi thao tác đều biến đổi hai số có khoảng cách hai giữa chúng thành số ở giữa. Điều này cho phép chúng tôi loại bỏ tất cả các giá trị lớn trong khi vẫn duy trì một chuỗi duy nhất mà cuối cùng đạt tới 2. 

Tại sao nó hoạt động: 

Bất biến là sau khi xử lý một số i, tất cả các giá trị lớn hơn i + 1 đã bị loại bỏ và bảng vẫn chứa giá trị i + 1. Hai thao tác đầu tiên thiết lập bất biến này cho các giá trị lớn nhất. Mọi thao tác tiếp theo đều bảo toàn nó vì kết hợp i và i + 2 sẽ tạo ra i + 1. Khi i đạt tới 1, thao tác cuối cùng kết hợp 1 và 3, tạo ra 2. Vì không thể có giá trị cuối cùng dưới 2 nên kết cấu là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    ans = ["2"]

    if n == 2:
        ans.append("1 2")
    else:
        ans.append(f"{n - 2} {n}")
        ans.append(f"{n - 1} {n - 1}")
        for i in range(n - 3, 0, -1):
            ans.append(f"{i} {i + 2}")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Chương trình lưu trữ các dòng đầu ra thay vì in sau mỗi thao tác. Điều này tránh được chi phí đầu ra không cần thiết trong khi vẫn chỉ sử dụng bộ nhớ tuyến tính cho câu trả lời được tạo. 

Trường hợp đặc biệt n = 2 được xử lý riêng vì cấu trúc tổng quát cần tồn tại các số n - 2 và n - 1. Đối với n lớn hơn, thao tác đầu tiên sử dụng n và n - 2, luôn là các giá trị hợp lệ. Vòng lặp bắt đầu ở n - 3 và dừng ở 1, tạo ra chính xác n - 3 thao tác bổ sung sau hai thao tác đầu tiên, với tổng số n - 1 thao tác. 

Tất cả số học nằm trong giới hạn số nguyên bình thường vì giá trị lớn nhất có liên quan là n, nhiều nhất là 200000. 

## Ví dụ đã hoạt động 

Với n = 4, thuật toán tạo ra: 

| Bước | Hoạt động | Kết quả có giá trị quan trọng | 
| --- | --- | --- | 
| Ban đầu | Giá trị 1, 2, 3, 4 | 4 | 
| 1 | Kết hợp 2 và 4 | 3 | 
| 2 | Kết hợp 3 và 3 | 3 | 
| 3 | Kết hợp 1 và 3 | 2 | 

Số cuối cùng là 2. Điều này thể hiện phần đầu tiên của công trình trong đó các giá trị lớn nhất được chuyển đổi thành một chuỗi dẫn xuống dưới. 

Với n = 5, thuật toán tạo ra: 

| Bước | Hoạt động | Kết quả có giá trị quan trọng | 
| --- | --- | --- | 
| Ban đầu | Giá trị 1, 2, 3, 4, 5 | 5 | 
| 1 | Kết hợp 3 và 5 | 4 | 
| 2 | Kết hợp 4 và 4 | 4 | 
| 3 | Kết hợp 2 và 4 | 3 | 
| 4 | Kết hợp 1 và 3 | 2 | 

Dấu vết này cho thấy cách duy trì tính bất biến. Sau hai thao tác đầu tiên, giá trị lớn còn lại giảm dần cho đến khi chỉ còn lại 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chính xác n - 1 thao tác được tạo ra. | 
| Không gian | O(n) | Bản thân đầu ra chứa n dòng, trong khi logic thuật toán sử dụng bộ nhớ bổ sung không đổi. | 

Các giới hạn yêu cầu một giải pháp tuyến tính vì n có thể đạt tới 200000. Thuật toán thực hiện một lượng công việc cố định nhỏ cho mỗi thao tác nên dễ dàng phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    ans = ["2"]

    if n == 2:
        ans.append("1 2")
    else:
        ans.append(f"{n - 2} {n}")
        ans.append(f"{n - 1} {n - 1}")
        for i in range(n - 3, 0, -1):
            ans.append(f"{i} {i + 2}")

    return "\n".join(ans)

assert solve("2\n") == "2\n1 2", "minimum n"
assert solve("4\n") == "2\n2 4\n3 3\n1 3", "sample style case"
assert solve("5\n") == "2\n3 5\n4 4\n2 4\n1 3", "chain construction"
assert solve("200000\n").splitlines()[0] == "2", "maximum size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | Giá trị cuối cùng 2 với một thao tác | Cho phép nhỏ nhất n | 
| 4 | Một công trình ba hoạt động hợp lệ | Mẫu ví dụ tiêu chuẩn | 
| 5 | Chuỗi giảm dài hơn | Bất biến qua nhiều bước | 
| 200000 | Dòng đầu tiên là 2 | Xử lý hạn chế tối đa | 

## Vỏ cạnh 

Với n = 2, bảng chỉ chứa 1 và 2. Thuật toán in ngay thao tác duy nhất có thể,`1 2`. Giá trị kết quả là ceil(3 / 2), bằng 2. 

Với n = 3, thuật toán không sử dụng trường hợp đặc biệt. Đầu tiên, nó kết hợp 1 và 3, tạo ra 2, sau đó kết hợp 2 và 2, giữ nguyên 2. Giá trị cuối cùng vẫn là 2 và việc xây dựng tránh được chiến lược sai lầm là giảm các số nhỏ nhất trước. 

Đối với n lớn, thuật toán không bao giờ lưu trữ bảng một cách rõ ràng. Nó chỉ in một chuỗi có giá trị được đảm bảo tồn tại tại thời điểm chúng được sử dụng. Vòng lặp giảm đảm bảo rằng không có cặp không hợp lệ nào được yêu cầu, ngay cả khi n đạt tới 200000.
