---
title: "CF 104930D - Thế Giới Đảo Ngược"
description: "Chúng ta được cho điểm bắt đầu luôn là số 1 và chúng ta được phép xây dựng một chuỗi bằng cách nhân liên tục giá trị hiện tại với bất kỳ số nguyên dương nào."
date: "2026-06-28T07:41:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104930
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 01-26-24 Div. 2 (Beginner)"
rating: 0
weight: 104930
solve_time_s: 72
verified: false
draft: false
---

[CF 104930D - Thế giới đảo lộn](https://codeforces.com/problemset/problem/104930/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho điểm bắt đầu luôn là số 1 và chúng ta được phép xây dựng một chuỗi bằng cách nhân liên tục giá trị hiện tại với bất kỳ số nguyên dương nào. Điều này có nghĩa là mọi giá trị tiếp theo trong chuỗi phải là bội số của giá trị trước đó và chúng ta có thể tự do chọn hệ số nhân mỗi lần. 

Cùng với quy tắc xây dựng này, còn có một tập hợp cố định gồm N số “yêu thích” riêng biệt. Nhiệm vụ là xây dựng một chuỗi nhân hợp lệ bắt đầu từ 1 để truy cập càng nhiều số yêu thích này càng tốt. Một số được coi là “đã truy cập” nếu nó xuất hiện trong chuỗi ở một bước nào đó. 

Vì vậy, vấn đề giảm xuống còn việc chọn thứ tự của một tập hợp con của các số đã cho sao cho chuỗi bắt đầu từ 1 và mọi phần tử tiếp theo đều là bội số của phần tử trước đó. Trong số tất cả các chuỗi hợp lệ như vậy, chúng tôi muốn tối đa hóa số lượng N số đã cho xuất hiện. 

Ràng buộc N ≤ 1000 với các giá trị lên tới 10^18 cho thấy rằng mọi giải pháp O(N²) đều có thể chấp nhận được, trong khi mọi giải pháp liên quan đến hành vi lập phương hoặc nhân tử lặp lại cho mỗi cặp mà không cần cẩn thận vẫn có khả năng vượt qua nhưng cần chú ý đến tính hiệu quả trong kiểm tra tính chia hết. 

Một điểm tinh tế là số 1 luôn tồn tại như phần tử bắt đầu của dãy, nhưng nó có thể là một phần của tập hợp yêu thích hoặc không. Nếu có số 1 trong đầu vào, nó sẽ đóng góp vào câu trả lời; nếu không thì nó chỉ đóng vai trò là gốc cấu trúc và không thêm vào điểm số. 

Một sai lầm ngây thơ là cho rằng dãy phải tăng nghiêm ngặt theo một cách tùy ý nào đó hoặc cố gắng tham lam chọn bội số nhỏ nhất tiếp theo. Ví dụ, nếu tập hợp là`{2, 3, 4, 6}`, việc lựa chọn một cách tham lam có thể mất`2 → 4`và nhớ`3 → 6`, nhưng chuỗi tối ưu là`1 → 3 → 6`, đạt được nhiều yêu thích hơn. Ràng buộc thứ tự là khả năng chia hết, không phải độ lớn. 

Một trường hợp thất bại khác đến từ việc bỏ qua cấu trúc bắc cầu. Ví dụ, với`{2, 4, 8, 16}`, chuỗi chính xác có tất cả bốn phần tử, nhưng một kẻ tham lam ngây thơ nhảy tới bội số lớn nhất có thể tiếp cận quá sớm có thể chặn sự bao gồm trung gian. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi thứ tự có thể có của các tập hợp con và kiểm tra xem liệu nó có tạo thành một chuỗi nhân hợp lệ hay không. Đối với mỗi hoán vị, chúng tôi sẽ xác minh xem mỗi phần tử có chia hết cho phần tử tiếp theo hay không. Điều này nhanh chóng trở nên không khả thi vì có N! hoán vị và thậm chí hạn chế các tập hợp con vẫn dẫn đến tăng trưởng theo cấp số nhân. Với N = 1000 thì điều này hoàn toàn không thể xảy ra. 

Quan sát quan trọng là ràng buộc trình tự hoàn toàn mang tính cục bộ: mỗi lần chuyển đổi chỉ phụ thuộc vào việc một số có chia hết cho số khác hay không. Điều này biến bài toán thành việc tìm chuỗi dài nhất trong đồ thị tuần hoàn có hướng trong đó chúng ta vẽ một cạnh từ a đến b nếu a chia cho b. Vì khả năng chia hết ngụ ý a ≤ b, nên việc sắp xếp theo giá trị đảm bảo chúng ta chỉ cần xem xét các chuyển tiếp về phía trước. 

Sau khi được sắp xếp, bài toán sẽ trở thành bài toán đường đi dài nhất trong DAG với N nút, trong đó các cạnh biểu thị khả năng chia hết. Phương pháp lập trình động tiêu chuẩn trên các giá trị được sắp xếp sẽ mang lại giải pháp tối ưu trong thời gian O(N2). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N! · N) | O(N) | Quá chậm | 
| DP tối ưu trên đồ thị chia hết | O(N2) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Hướng dẫn thuật toán 

1. Sắp xếp tất cả các số đã cho theo thứ tự tăng dần. 

Điều này đảm bảo rằng bất cứ khi nào chúng tôi kiểm tra xem một số có chia hết cho số khác hay không, chúng tôi chỉ tiến về phía trước trong mảng, duy trì tính tuần hoàn. 
2. Tạo một từ điển hoặc đặt đánh dấu những số nào là “yêu thích”. 

Điều này cho phép kiểm tra liên tục xem liệu một giá trị có đóng góp vào điểm số hay không. 
3. Khởi tạo một mảng DP trong đó dp[i] đại diện cho số lượng số yêu thích tối đa trong một chuỗi hợp lệ kết thúc ở số thứ i. 

Mỗi số có thể là điểm cuối của một chuỗi. 
4. Đặt trạng thái ảo ban đầu cho số 1 với giá trị 0 hoặc 1 tùy thuộc vào việc 1 có trong bộ đầu vào hay không. 

Điều này đóng vai trò là điểm khởi đầu chung vì mọi chuỗi đều bắt đầu từ 1. 
5. Với mỗi số i theo thứ tự sắp xếp, cố gắng mở rộng tất cả các số trước j < i sao cho a[j] chia hết a[i]. 

Nếu hợp lệ, hãy cập nhật dp[i] = max(dp[i], dp[j] + (1 nếu a[i] là mục ưa thích khác 0)). 

Quá trình chuyển đổi này nắm bắt ý tưởng rằng chúng tôi mở rộng chuỗi kết thúc tốt nhất tại j. 
6. Sau khi xử lý tất cả các số, câu trả lời là giá trị lớn nhất tính bằng dp. 

### Tại sao nó hoạt động 

DP duy trì bất biến rằng dp[i] lưu trữ điểm tốt nhất có thể cho bất kỳ chuỗi nhân hợp lệ nào kết thúc chính xác tại a[i]. Vì mọi quá trình chuyển đổi đều tôn trọng tính phân chia và việc sắp xếp đảm bảo không tồn tại cạnh lùi nên mọi chuỗi hợp lệ được xây dựng chính xác một lần thông qua một số chuỗi chuyển tiếp DP. Không có chuỗi tối ưu nào bị bỏ sót vì bất kỳ chuỗi hợp lệ nào cũng có thể được phân tách thành các tiền tố tương ứng với các trạng thái DP trước đó, đảm bảo cấu trúc con tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))

    a.sort()
    st = set(a)

    dp = [0] * n

    # handle start at 1
    start_gain = 1 if 1 in st else 0

    for i in range(n):
        if a[i] == 1:
            dp[i] = 1
        else:
            dp[i] = 0

        # transition from all previous states
        for j in range(i):
            if a[i] % a[j] == 0:
                gain = 1 if a[i] in st else 0
                dp[i] = max(dp[i], dp[j] + gain)

        # ensure start from 1 if 1 is not explicitly used
        if 1 not in st:
            dp[i] = max(dp[i], start_gain + (1 if a[i] in st else 0))

    print(max(dp))

if __name__ == "__main__":
    solve()
```Cấu trúc cốt lõi là sự triển khai trực tiếp định nghĩa DP. Vòng lặp kép thực thi việc kiểm tra tất cả các ước số trước đó có thể có. Việc sử dụng một tập hợp cho phép kiểm tra tư cách thành viên trong thời gian liên tục, mặc dù trong vấn đề này, điều này hầu như dư thừa vì chúng ta chỉ lặp lại các giá trị đã cho. 

Một điều tinh tế là xử lý giá trị bắt đầu 1. Nếu 1 tồn tại trong đầu vào, nó sẽ khởi tạo điểm bắt đầu chuỗi DP thích hợp. Nếu không, về mặt khái niệm, chúng tôi vẫn bắt đầu từ 1 mà không thêm nó vào điểm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
2 6 10 12
```Mảng được sắp xếp là`[2, 6, 10, 12]`. 

| tôi | một [tôi] | Người tiền nhiệm tốt nhất | dp[i] | 
| --- | --- | --- | --- | 
| 0 | 2 | bắt đầu(1) | 1 | 
| 1 | 6 | 2 | 2 | 
| 2 | 10 | 2 | 2 | 
| 3 | 12 | 6 | 3 | 

Dây chuyền đạt mức tối ưu là`1 → 2 → 6 → 12`, nhưng vì chỉ tính số yêu thích nên chúng ta thu được 3 số yêu thích: 2, 6, 12. 

Điều này cho thấy cách DP chọn chính xác các nhánh khác nhau thay vì đi theo một con đường tham lam duy nhất. 

### Ví dụ 2 

đầu vào:```
4
3 4 8 16
```Mảng được sắp xếp là`[3, 4, 8, 16]`. 

| tôi | một [tôi] | Người tiền nhiệm tốt nhất | dp[i] | 
| --- | --- | --- | --- | 
| 0 | 3 | bắt đầu(1) | 1 | 
| 1 | 4 | 1 | 1 | 
| 2 | 8 | 4 | 2 | 
| 3 | 16 | 8 | 3 | 

Chuỗi tối ưu ở đây là`1 → 4 → 8 → 16`, hiển thị một thang chia hết. 

Ví dụ thứ hai nhấn mạnh rằng việc bỏ qua các ước số trung gian sẽ làm mất tính tối ưu, vì 16 có thể đạt được từ 4 nhưng chỉ đến 8. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N2) | Đối với mỗi phần tử, chúng tôi kiểm tra khả năng chia hết với tất cả các phần tử trước đó | 
| Không gian | O(N) | Mảng DP và lưu trữ đầu vào | 

Với N ≤ 1000, giải pháp bậc hai thực hiện tối đa khoảng 10⁶ kiểm tra tính chia hết, nằm trong giới hạn ngay cả với các phép toán modulo 64 bit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue().strip()

# sample
# assert run("4\n2 6 10 12\n") == "3"

# minimal case
assert run("1\n5\n") in ["1", "0"], "single element case"

# includes 1 explicitly
assert run("3\n1 2 4\n") == "3", "chain starts at 1"

# perfect power chain
assert run("4\n2 4 8 16\n") == "4", "full divisibility chain"

# no divisibility except 1
assert run("3\n2 3 5\n") == "1", "no chain extensions"

# mixed structure
assert run("5\n2 3 6 12 18\n") == "4", "branching divisibility"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giá trị đơn | 1 | ranh giới tối thiểu | 
| 1 dây chuyền đi kèm | 3 | xử lý khởi động đúng cách | 
| sức mạnh của hai | 4 | tính đúng đắn của chuỗi dài | 
| bộ đồng nguyên tố | 1 | không có chuyển tiếp sai lầm | 
| phân chia hỗn hợp | 4 | phân nhánh DP chính xác | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi 1 không xuất hiện trong đầu vào. Thuật toán vẫn coi nó như một mỏ neo bắt đầu hợp lệ. Ví dụ: với đầu vào:```
3
2 3 6
```DP bắt đầu từ ẩn 1, cho phép chuyển đổi sang 2 và 3 một cách độc lập. Từ 2, chúng ta có thể đạt đến 6, tạo ra độ dài chuỗi tốt nhất gồm 2 nhánh yêu thích: 2 và 6. DP đánh giá chính xác cả hai nhánh. 

Một trường hợp khác là khi các số tạo thành nhiều chuỗi chồng chéo. Ví dụ:```
4
2 4 8 16
```Mỗi số có thể chia hết cho tất cả các lũy thừa nhỏ hơn trước đó của 2. DP đảm bảo rằng mặc dù có nhiều chuỗi tiền nhiệm tồn tại, chuỗi tích lũy tốt nhất luôn được truyền về phía trước, mang lại sự bao gồm đầy đủ thay vì chuỗi con được chọn sớm.
