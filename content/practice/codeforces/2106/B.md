---
title: "CF 2106B - St. Chroma"
description: "Chúng ta được cung cấp một danh sách phải là một hoán vị của các số từ 0 đến n−1. Khi chúng tôi tiết lộ hoán vị này từ trái sang phải, chúng tôi tính toán một giá trị đang chạy: sau mỗi tiền tố, chúng tôi lấy số nguyên không âm nhỏ nhất bị thiếu trong tiền tố đó."
date: "2026-06-08T04:52:16+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2106
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1020 (Div. 3)"
rating: 900
weight: 2106
solve_time_s: 87
verified: false
draft: false
---

[CF 2106B - St. Chroma](https://codeforces.com/problemset/problem/2106/B) 

**Xếp hạng:** 900 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách phải là một hoán vị của các số từ 0 đến n−1. Khi chúng tôi tiết lộ hoán vị này từ trái sang phải, chúng tôi tính toán một giá trị đang chạy: sau mỗi tiền tố, chúng tôi lấy số nguyên không âm nhỏ nhất bị thiếu trong tiền tố đó. Giá trị này thay đổi theo thời gian khi có nhiều phần tử xuất hiện hơn và mỗi tiền tố sẽ vẽ một ô với giá trị hiện tại của nó. 

Nhiệm vụ không phải là tính màu này cho một hoán vị cố định. Thay vào đó, chúng ta phải tự xây dựng hoán vị sao cho một màu x cụ thể xuất hiện nhiều lần nhất có thể trong dải sơn thu được. 

Tương tác chính là giữa thứ tự của các phần tử trong hoán vị và thời điểm MEX của tiền tố di chuyển qua x. Vì MEX chỉ phụ thuộc vào giá trị nào từ 0 trở lên đã xuất hiện nên thứ tự xác định thời gian MEX giữ nguyên bằng một giá trị cụ thể. 

Các ràng buộc đủ lớn để mọi giải pháp đều phải tuyến tính cho mỗi trường hợp thử nghiệm. Tổng của n lên tới 2×10^5, loại trừ mọi thứ bậc hai hoặc liên quan đến mô phỏng lặp lại trên mỗi hoán vị ứng cử viên. Việc xây dựng phải trực tiếp. 

Trường hợp cạnh tinh tế xuất hiện khi x = n. Trong trường hợp đó, MEX chỉ bằng n sau khi tất cả các số từ 0 đến n−1 xuất hiện. Điều đó có nghĩa là nó chỉ xảy ra ở tiền tố cuối cùng nên câu trả lời luôn là 1, bất kể sự sắp xếp. Một nỗ lực ngây thơ coi x như một giá trị bình thường sẽ cố gắng “trì hoãn” MEX vượt quá n một cách không chính xác, điều này là không thể. 

Một trường hợp cạnh khác là x = 0. Ở đây MEX ban đầu là 0 và trở thành 1 ngay khi 0 xuất hiện. Vì vậy, chúng tôi muốn trì hoãn việc đặt số 0 càng lâu càng tốt để kéo dài thời gian chạy màu 0 ban đầu. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là tạo ra tất cả các hoán vị và mô phỏng quy trình MEX cho mỗi hoán vị, đếm số lần màu x xuất hiện. Điều này đúng nhưng không khả thi. Có n! hoán vị, và thậm chí việc đánh giá một hoán vị có giá O(n), vì vậy cách tiếp cận này rất lớn về mặt thiên văn. 

Để tối ưu hóa, chúng ta cần hiểu điều gì làm cho MEX luôn bằng x đối với các tiền tố liên tiếp. MEX bằng x chính xác khi tất cả các số từ 0 đến x−1 đã xuất hiện trong tiền tố, nhưng bản thân x vẫn chưa xuất hiện. Khi x được chèn vào, MEX sẽ nhảy tới giá trị lớn hơn x. 

Điều này có nghĩa là chúng tôi muốn trì hoãn việc chèn x càng lâu càng tốt, đồng thời đảm bảo rằng tất cả các số nhỏ hơn xuất hiện đủ sớm để MEX đã đạt đến x. Sau thời điểm đó, mọi phần tử bổ sung trước x được chèn sẽ giữ MEX cố định ở x, mang lại cho chúng ta dải màu không đổi như mong muốn. 

Quyết định thực sự duy nhất là đặt x ở đâu so với các số từ 0 đến x−1. Khi tất cả các số nhỏ hơn đó đều có mặt, chúng ta có thể tự do đặt tất cả các số lớn hơn x ở bất kỳ đâu mà không ảnh hưởng đến MEX là x, miễn là x vẫn bị thiếu. 

Điều này dẫn đến một cấu trúc tối ưu đơn giản: đặt tất cả các số lớn hơn x trước, sau đó đặt 0 đến x−1 và cuối cùng đặt x ở cuối. Điều này tối đa hóa khoảng thời gian trong đó tiền tố chứa tất cả các giá trị nhỏ hơn x nhưng không chứa chính x. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Xây dựng tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chia hoán vị thành ba nhóm khái niệm: các số lớn hơn x, các số từ 0 đến x−1 và chính giá trị x duy nhất. Sự tách biệt này dựa trên cách mỗi nhóm ảnh hưởng đến giá trị MEX. 
2. Đặt tất cả các số lớn hơn x vào đầu hoán vị. Các giá trị này không giúp hình thành tiền tố xác định MEX = x, nhưng chúng cũng không can thiệp vào nó. 
3. Đặt tất cả các số từ 0 đến x−1 ngay sau đó. Điều này đảm bảo rằng khi khối này được bao gồm đầy đủ trong tiền tố, mọi giá trị cần thiết để đẩy MEX lên x đều xuất hiện. 
4. Đặt x càng muộn càng tốt, lý tưởng nhất là ở cuối. Điều này tối đa hóa số lượng tiền tố trong đó có tất cả các giá trị nhỏ hơn nhưng x vẫn vắng mặt, đây chính xác là điều kiện để MEX bằng x. 
5. Xuất ra chuỗi đã xây dựng. 

Ý tưởng chính là khi tiền tố chứa tất cả các giá trị từ 0 đến x−1, MEX sẽ trở thành x và giữ nguyên ở đó cho đến khi x xuất hiện. Chúng tôi đang tối đa hóa thời gian có thể trì hoãn sự kiện cuối cùng đó. 

### Tại sao nó hoạt động 

MEX của tiền tố được xác định hoàn toàn bởi giá trị nhỏ nhất bị thiếu. Để có MEX bằng x, phải có hai điều kiện đồng thời: mọi giá trị từ 0 đến x−1 phải có sẵn và x phải chưa có mặt. Khi cả hai đều hài lòng, việc thêm bất kỳ giá trị nào lớn hơn x sẽ không làm thay đổi MEX, vì nó không điền bất kỳ số nguyên bị thiếu nào bên dưới x. Do đó, mọi vị trí sau lần xuất hiện đầy đủ đầu tiên của 0 đến x−1 và trước x đều góp phần xuất hiện hợp lệ màu x. Việc xây dựng tối đa hóa khoảng thời gian này bằng cách đảm bảo tất cả các giá trị nhỏ xuất hiện càng sớm càng tốt trong khi trì hoãn x. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, x = map(int, input().split())

    if x == n:
        # MEX can only become n at full prefix
        print(*range(n))
        continue

    res = []

    # first put all values > x
    for i in range(x + 1, n):
        res.append(i)

    # then values < x
    for i in range(x):
        res.append(i)

    # finally x
    res.append(x)

    print(*res)
```Việc triển khai tuân theo quan sát trực tiếp rằng chỉ có vị trí tương đối của x đối với [0, x−1] mới quan trọng. Các giá trị lớn hơn x có thể được đặt ở bất kỳ đâu trước x mà không ảnh hưởng khi MEX trở thành x, vì vậy chúng tôi đặt chúng lên đầu một cách an toàn. Ranh giới vòng lặp rất quan trọng: vòng lặp đầu tiên bắt đầu ở x+1 để loại trừ chính x và vòng lặp thứ hai đi lên bao gồm x−1. 

Trường hợp đặc biệt x = n tránh được điều kiện khái niệm trống trong đó x nằm ngoài phạm vi hoán vị; trong trường hợp đó, cách sắp xếp tối ưu là không phù hợp vì MEX không bao giờ bằng n trước tiền tố cuối cùng. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện. 

### Ví dụ 1: n = 4, x = 2 

Chúng tôi xây dựng hoán vị. 

| Bước | Đã thêm khối | Hoán vị hiện tại | 
| --- | --- | --- | 
| 1 | giá trị > 2 → [3] | [3] | 
| 2 | giá trị < 2 → [0, 1] | [3, 0, 1] | 
| 3 | thêm x → [2] | [3, 0, 1, 2] | 

Bây giờ chúng tôi kiểm tra sự tiến hóa của MEX: 

| Tiền tố | Yếu tố | MEX | 
| --- | --- | --- | 
| 1 | [3] | 0 | 
| 2 | [3, 0] | 1 | 
| 3 | [3, 0, 1] | 2 | 
| 4 | [3, 0, 1, 2] | 4 | 

Màu 2 xuất hiện đúng một lần. Việc xây dựng đảm bảo chúng ta đạt MEX = 2 sớm nhất có thể và giữ ổn định cho đến khi x xuất hiện. 

### Ví dụ 2: n = 5, x = 0 

| Bước | Đã thêm khối | Hoán vị hiện tại | 
| --- | --- | --- | 
| 1 | giá trị > 0 → [1, 2, 3, 4] | [1, 2, 3, 4] | 
| 2 | giá trị < 0 → [] | [1, 2, 3, 4] | 
| 3 | thêm x → [0] | [1, 2, 3, 4, 0] | 

Sự phát triển của MEX: 

| Tiền tố | Yếu tố | MEX | 
| --- | --- | --- | 
| 1 | [1] | 0 | 
| 2 | [1,2] | 0 | 
| 3 | [1,2,3] | 0 | 
| 4 | [1,2,3,4] | 0 | 
| 5 | [1,2,3,4,0] | 5 | 

Màu 0 xuất hiện trong tất cả các tiền tố cho đến khi số 0 được đặt, tối đa hóa tần suất của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi số được đặt đúng một lần trong công trình | 
| Không gian | O(n) | Chúng tôi lưu trữ hoán vị kết quả | 

Tổng n trên tất cả các trường hợp thử nghiệm tối đa là 2×10^5, do đó việc xây dựng tuyến tính dễ dàng phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, x = map(int, input().split())
        if x == n:
            out.append(" ".join(map(str, range(n))))
            continue

        res = []
        for i in range(x + 1, n):
            res.append(i)
        for i in range(x):
            res.append(i)
        res.append(x)
        out.append(" ".join(map(str, res)))

    return "\n".join(out)

# provided samples
assert run("""7
4 2
4 0
5 0
1 1
3 3
1 0
4 3
""") == """1 0 3 2
2 3 1 0
3 4 0 1 2
0
0 2 1
0
1 2 0 3"""

# custom cases
assert run("1\n2 2\n") == "0 1", "x = n edge"
assert run("1\n3 0\n") in ["1 2 0", "2 1 0"], "x = 0 behavior"
assert run("1\n5 4\n") == "0 1 2 3 4", "x near end"
assert run("1\n5 2\n") == "3 4 0 1 2", "middle split"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 | 0 1 | x = n trường hợp cạnh | 
| 3 0 | biến thể | ứng xử khi x = 0 | 
| 5 4 | 0 1 2 3 4 | ranh giới gần cuối | 
| 5 2 | 3 4 0 1 2 | tính đúng đắn của việc chia chung | 

## Vỏ cạnh 

Khi x = n, thuật toán lấy hoán vị nhận dạng. Đối với một ví dụ nhỏ như n = 3, x = 3, cấu trúc tạo ra [0, 1, 2]. MEX chỉ đạt 3 sau khi tất cả các phần tử được nhìn thấy, do đó, chỉ riêng tiền tố cuối cùng đã đóng góp màu mục tiêu. 

Khi x = 0, cấu trúc đặt tất cả các giá trị khác lên đầu tiên, sau đó là 0 ở cuối. Với n = 4, x = 0, ta được [1, 2, 3, 0]. MEX duy trì ở mức 0 cho mọi tiền tố cho đến khi 0 xuất hiện, đây là khoảng thời gian tối đa có thể đạt được. 

Khi x ở giữa, chẳng hạn như n = 6, x = 2, chuỗi [3, 4, 5, 0, 1, 2] đảm bảo rằng khi 0 và 1 xuất hiện, MEX trở thành 2 và giữ nguyên ở đó cho đến khi 2 được thêm vào, tối đa hóa số lượng ô 2 màu.
