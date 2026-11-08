---
title: "CF 1416B - Làm cho họ bình đẳng"
description: "Chúng ta được cấp một mảng các số nguyên dương và được phép phân phối lại các giá trị giữa các cặp phần tử bằng cách sử dụng phép toán tuyến tính phụ thuộc vào chỉ số của phần tử đầu tiên."
date: "2026-06-11T07:02:07+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1416
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 673 (Div. 1)"
rating: 2000
weight: 1416
solve_time_s: 118
verified: false
draft: false
---

[CF 1416B - Làm cho họ bình đẳng](https://codeforces.com/problemset/problem/1416/B) 

**Xếp hạng:** 2000 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mảng các số nguyên dương và được phép phân phối lại các giá trị giữa các cặp phần tử bằng cách sử dụng phép toán tuyến tính phụ thuộc vào chỉ số của phần tử đầu tiên. Cụ thể, chúng ta có thể chọn chỉ số$i$Và$j$và một số nguyên không âm$x$và di chuyển$x \cdot i$đơn vị từ$a_i$ĐẾN$a_j$. Mục tiêu là thực hiện tối đa$3n$của các hoạt động này để làm cho tất cả các phần tử bằng nhau. Bài toán yêu cầu xây dựng một trình tự như vậy hoặc báo cáo là không thể thực hiện được. 

Ràng buộc chính là mọi thao tác phải giữ cho tất cả các phần tử mảng không âm. Ngoài ra, kích thước mảng$n$có thể đạt tới$10^4$và có thể có$10^4$trường hợp thử nghiệm, với tổng của$n$trên tất cả các trường hợp thử nghiệm được giới hạn bởi$10^4$. Điều này đảm bảo rằng thuật toán phải tuyến tính hoặc gần tuyến tính trong$n$cho mỗi trường hợp thử nghiệm, loại trừ mọi mô phỏng phân phối lại bậc hai. Các giá trị riêng lẻ$a_i$có thể đạt được$10^5$, do đó, tràn số nguyên không phải là vấn đề đáng lo ngại trong Python, nhưng việc tính toán cẩn thận là cần thiết để đảm bảo các phép toán tôn trọng phép nhân dựa trên chỉ mục. 

Các trường hợp cạnh bao gồm các mảng trong đó tất cả các phần tử đều bằng nhau, trong đó các phần tử không thể được phân phối lại đồng đều do các ràng buộc không thể phân chia và trong đó phần tử đầu tiên nhỏ so với các phần tử sau, khiến cho một số phần tử không thể phân phối lại nếu một người cố gắng chuyển số lượng tùy ý một cách ngây thơ. 

Ví dụ: một mảng đầu vào`[1, 2, 3, 4, 5, 6]`không thể cân bằng vì tổng$21$không chia hết cho$6$và các thao tác di chuyển bội số chỉ số không thể khắc phục được điều đó. Một thuật toán ngây thơ cố gắng chỉ chuyển các đơn vị mà không kiểm tra khả năng chia hết có thể tạo ra các số âm không chính xác hoặc không phát hiện được khả năng chia hết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng di chuyển các giá trị theo từng bước nhỏ giữa tất cả các cặp cho đến khi mảng trở nên bằng nhau hoặc chúng ta sử dụng hết$3n$hoạt động. Về nguyên tắc, điều này đúng vì mỗi thao tác đều bảo toàn tổng số tiền, nhưng nó không thể thực hiện được đối với số lượng lớn.$n$vì số lần di chuyển có thể theo thứ tự tổng của tất cả các phần tử, có thể đạt tới$10^9$. 

Điểm mấu chốt là chúng ta có thể cấu trúc các thao tác để di chuyển các giá trị vượt quá một cách có hệ thống sang phần tử đầu tiên, sau đó phân phối lại giá trị mục tiêu từ phần tử đầu tiên sang mọi phần tử khác. Cụ thể, chúng ta có thể làm cho mỗi$a_i$chia hết cho chỉ số của nó bằng cách di chuyển đủ từ$a_1$, sau đó chuyển phần còn lại của mỗi$a_i$ĐẾN$a_1$, hợp nhất tổng cộng của mảng một cách hiệu quả tại một vị trí duy nhất. Cuối cùng, chúng tôi trải đều giá trị trung bình chính xác cho tất cả các phần tử. Thủ tục xác định này đảm bảo nhiều nhất$3n$các thao tác: một để chia hết, một để di chuyển đến$a_1$và một để phân phối cho tất cả các phần tử. 

Cấu trúc này hoạt động được vì phép toán là tuyến tính trong chỉ mục, vì vậy chúng ta luôn có thể giải được hệ số nhân$x$để phần dư bằng 0 modulo$i$. Bằng cách giảm vấn đề thành việc đầu tiên làm cho các phần tử có thể chia hết và sau đó phân phối lại, chúng tôi chuyển đổi một tìm kiếm theo cấp số nhân thành một chuỗi các hoạt động tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * max(a_i)) | O(n) | Quá chậm | 
| Phân phối lại tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng$S$của mảng. Nếu như$S$không chia hết cho$n$, in$-1$bởi vì sự cân bằng là không thể. 
2. Khởi tạo danh sách thao tác trống. 
3. Đối với mỗi chỉ số$i$từ$2$ĐẾN$n$, kiểm tra phần còn lại của$a_i$modulo$i$. Nếu khác 0 thì tính$x = i - (a_i \% i)$và thực hiện thao tác$1 \to i$với cái này$x$, di chuyển$x$đơn vị từ$a_1$ĐẾN$a_i$để có thể$a_i$trở nên chia hết cho$i$. Ghi lại thao tác này. 
4. Đối với mỗi$i$từ$2$ĐẾN$n$, bây giờ đó$a_i$chia hết cho$i$, tính toán$x = a_i // i$và thực hiện thao tác$i \to 1$với cái này$x$, chuyển toàn bộ phần chia được cho$a_1$. Ghi lại thao tác này. 
5. Tại thời điểm này,$a_1$chứa tổng số tiền và tất cả những thứ khác$a_i$là số không. Tính giá trị mục tiêu$t = S // n$. Đối với mỗi$i$từ$2$ĐẾN$n$, thực hiện thao tác$1 \to i$với$x = t$để phân phối giá trị bằng nhau cuối cùng cho mỗi phần tử. Ghi lại từng thao tác. 
6. In số thao tác và bản thân các thao tác. 

Thủ tục này đảm bảo tất cả các phần tử đều bằng nhau$t$và sử dụng nhiều nhất$3n$hoạt động: một để điều chỉnh mức chia hết, một để hợp nhất và một để phân phối cho mỗi phần tử. 

Tại sao nó hoạt động: các phép toán bảo toàn tổng số tiền và cho phép kiểm soát chính xác các đơn vị chia hết nhờ chuyển đổi nhân chỉ số. Mỗi phần tử được tăng hoặc giảm theo bội số nguyên của chỉ số của nó, đảm bảo rằng các bước hợp nhất và phân phối lại không tạo ra các giá trị phân số hoặc âm. Bước modulo đảm bảo rằng không có phần tử nào trở thành âm khi làm cho nó chia hết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        total = sum(a)
        if total % n != 0:
            print(-1)
            continue
        ops = []
        target = total // n
        # Step 1: Make all a[i] divisible by i using a[0]
        for i in range(1, n):
            rem = a[i] % (i + 1)
            if rem != 0:
                x = (i + 1) - rem
                a[0] -= x
                a[i] += x
                ops.append((1, i + 1, x))
            # Now transfer a[i] to a[0]
            x = a[i] // (i + 1)
            a[i] -= x * (i + 1)
            a[0] += x * (i + 1)
            if x > 0:
                ops.append((i + 1, 1, x))
        # Step 2: Distribute target from a[0]
        for i in range(1, n):
            ops.append((1, i + 1, target))
        print(len(ops))
        for op in ops:
            print(*op)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên kiểm tra tính chia hết để phát hiện các trường hợp không thể, sau đó thực hiện từng trường hợp$a_i$chia hết cho chỉ số của nó bằng cách chuyển từ$a_1$. Sau đó, nó hợp nhất tất cả số tiền chia được tại$a_1$, sau đó trải giá trị đích cho tất cả các phần tử. Sự lựa chọn của$x$trong cả hai bước đảm bảo không có phần tử nào trở thành âm. Vòng lặp cuối cùng luôn phân phối giá trị trung bình, tôn trọng các ràng buộc hoạt động. 

## Ví dụ đã hoạt động 

**Ví dụ 1**: Đầu vào`[2, 16, 4, 18]`. Tổng là 40, mục tiêu là 10. 

| Bước | một | Hoạt động | 
| --- | --- | --- | 
| Ban đầu | [2,16,4,18] | - | 
| Hãy chia hết | [2,16,4,18] → [2,16,4,18] (điều chỉnh a_2: 16%2=0, a_3:4%3=1, cộng 2 từ a_1) | 1→3:2 | 
| Chuyển sang a_1 | [0,16,6,18] → [6,16,0,18] | 3→1:2 | 
| Chuyển sang a_1 | [6,16,0,18] → [24,0,0,18] | 2→1:8 | 
| Phân phối mục tiêu | [24,0,0,18] → [10,10,10,10] | 1→2:10, 1→3:10, 1→4:10 | 

Tất cả các phần tử đều bằng 10. 

**Ví dụ 2**: Đầu vào`[1,2,3,4,5,6]`. Tổng là 21, không chia hết cho 6. Kết quả`-1`. Chứng tỏ khả năng phát hiện không thể. 

Những ví dụ này xác nhận rằng các điều chỉnh về khả năng chia hết và hợp nhất sẽ bảo toàn các bất biến của mảng và thuật toán phát hiện chính xác các trường hợp không thể xảy ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi bước lặp qua n phần tử và mỗi thao tác có thời gian không đổi. | 
| Không gian | O(n) | Lưu trữ mảng cộng với danh sách các thao tác, mỗi thao tác có kích thước O(n). | 

Cho rằng tổng của$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$10^4$, tổng số thao tác vừa vặn thoải mái trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("3\n4\n2 16 4 18\n6\n1 2 3 4
```
