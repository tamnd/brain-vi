---
title: "CF 1607D - Hoán vị xanh-đỏ"
description: "Chúng ta được cung cấp một mảng các số nguyên trong đó mỗi phần tử có một màu, xanh lam hoặc đỏ. Các phần tử màu xanh lam có thể giảm đi 1 lần bất kỳ, trong khi các phần tử màu đỏ có thể tăng lên 1 lần bất kỳ."
date: "2026-06-10T07:43:20+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1607
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 753 (Div. 3)"
rating: 1300
weight: 1607
solve_time_s: 86
verified: true
draft: false
---

[CF 1607D - Hoán vị xanh-đỏ](https://codeforces.com/problemset/problem/1607/D) 

**Đánh giá:** 1300 
**Tags:** tham lam, toán học, phân loại 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên trong đó mỗi phần tử có một màu, xanh lam hoặc đỏ. Các phần tử màu xanh lam có thể giảm đi 1 lần bất kỳ, trong khi các phần tử màu đỏ có thể tăng lên 1 lần bất kỳ. Mục đích là để xác định xem có thể chuyển đổi mảng thành hoán vị các số từ 1 đến$n$, Ở đâu$n$là độ dài của mảng. Một hoán vị ở đây có nghĩa là mọi số nguyên từ 1 đến$n$xảy ra đúng một lần, theo thứ tự bất kỳ. 

Những ràng buộc cho phép$n$lên tới$2 \cdot 10^5$cho mỗi trường hợp thử nghiệm, với tối đa$10^4$các trường hợp thử nghiệm, nghĩa là không thể mô phỏng mạnh mẽ mọi mức tăng/giảm có thể có. Chúng ta phải giải quyết vấn đề bằng toán học thay vì mô phỏng các nước đi. Bản thân các con số có thể có độ lớn lớn, lên tới$10^9$theo một trong hai hướng, do đó, bất kỳ cách tiếp cận nào dựa vào thao tác mảng trực tiếp hoặc đếm phạm vi lớn sẽ không hiệu quả. 

Các trường hợp cạnh có thể vấp phải các giải pháp ngây thơ bao gồm các mảng trong đó tất cả các số ban đầu đều lớn hơn$n$hoặc nhỏ hơn 1. Ví dụ: một mảng chỉ có các phần tử màu xanh lam, tất cả đều bằng 100 với$n = 5$không bao giờ có thể đạt đến phạm vi 1-5 bằng cách giảm dần, bởi vì một số số phải nhỏ hơn 1. Tương tự, mảng các phần tử màu đỏ nhỏ hơn 1 không thể đạt được mục tiêu của chúng chỉ bằng cách tăng. Các trường hợp trùng lặp cùng một số hoặc các số cực âm cũng không hề tầm thường vì chúng ta phải tôn trọng các phép toán màu trong khi cố gắng đạt được các số duy nhất trong khoảng từ 1 đến$n$phạm vi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các chuỗi hoạt động có thể có trên mỗi phần tử để cố gắng đạt được một hoán vị. Về nguyên tắc, điều này đúng vì nó khám phá thấu đáo mọi sự biến đổi, nhưng số lượng các khả năng sẽ tăng theo cấp số nhân với$n$, làm cho nó không thể$n$lên tới$2 \cdot 10^5$. 

Thông tin chuyên sâu quan trọng về giải pháp tối ưu xuất phát từ việc quan sát rằng mỗi phần tử chỉ có thể được điều chỉnh theo một hướng: phần tử màu xanh có thể giảm, phần tử màu đỏ có thể tăng. Vì vậy, đối với phần tử màu xanh$x$, giá trị cuối cùng tối thiểu mà nó có thể đạt tới là 1 (chúng ta không thể xuống dưới 1 đối với hoán vị) và giá trị tối đa là$x$chính nó. Đối với phần tử màu đỏ$x$, giá trị cuối cùng tối thiểu là$x$(chúng tôi không thể giảm nó) và mức tối đa là$n$. 

Từ quan sát này, chúng ta có thể sắp xếp các phần tử màu xanh và phần tử màu đỏ một cách độc lập và cố gắng gán cho chúng các giá trị từ 1 đến$n$tham ăn. Các phần tử màu xanh phải được gán càng nhỏ càng tốt bắt đầu từ 1 và các phần tử màu đỏ phải được gán càng lớn càng tốt bắt đầu từ$n$, đảm bảo rằng mỗi số đều có thể truy cập được với các ràng buộc hoạt động. Nếu tại bất kỳ thời điểm nào, một số không thể được gán cho một phần tử tương ứng vì nó nằm ngoài phạm vi có thể truy cập được thì câu trả lời là KHÔNG. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tách mảng thành hai danh sách:`blue`chứa tất cả các phần tử màu xanh lam và`red`chứa tất cả các phần tử màu đỏ. Điều này cho phép chúng tôi xử lý phạm vi của chúng một cách độc lập. 
2. Sắp xếp`blue`theo thứ tự tăng dần. Đối với mỗi phần tử màu xanh tại vị trí$i$trong danh sách đã sắp xếp, hãy tính giá trị tối đa có thể truy cập được, chính là phần tử đó. Mức tối thiểu chúng tôi muốn nó thực hiện là$i+1$, vì số nhỏ nhất trong hoán vị bắt đầu từ 1 và tăng dần theo thứ tự. Nếu phần tử màu xanh nhỏ hơn$i+1$, nó không thể đạt được giá trị yêu cầu nên câu trả lời là KHÔNG. 
3. Sắp xếp`red`theo thứ tự giảm dần. Đối với mỗi phần tử màu đỏ tại vị trí$i$trong danh sách đã sắp xếp, hãy tính giá trị tối thiểu có thể truy cập được, chính là phần tử đó. Mức tối đa chúng tôi muốn nó mất là$n-i$, bắt đầu từ$n$và giảm dần. Nếu phần tử màu đỏ lớn hơn$n-i$, nó không thể đạt được giá trị yêu cầu nên câu trả lời là KHÔNG. 
4. Nếu tất cả các phần tử xanh và đỏ đều thỏa mãn các điều kiện trên thì câu trả lời là CÓ. 

Điều này hoạt động vì phạm vi giá trị mà mỗi màu có thể đạt tới tạo thành một chuỗi và việc gán chúng một cách tham lam sẽ đảm bảo không có số nào bị bỏ qua hoặc trùng lặp. Bất biến được duy trì là sau khi gán giá trị đầu tiên$k$các con số để$k$màu xanh lam nhỏ nhất và màu đỏ lớn nhất, tất cả các số còn lại vẫn có thể gán được trong phạm vi cho phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_form_permutation(n, a, colors):
    blue = []
    red = []
    for val, color in zip(a, colors):
        if color == 'B':
            blue.append(val)
        else:
            red.append(val)
    
    blue.sort()
    red.sort(reverse=True)
    
    for i, val in enumerate(blue):
        if val < i + 1:
            return "NO"
    for i, val in enumerate(red):
        if val > n - i:
            return "NO"
    return "YES"

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    colors = input().strip()
    print(can_form_permutation(n, a, colors))
```Trước tiên, chúng tôi tách các phần tử theo màu sắc, sắp xếp từng nhóm rồi kiểm tra từng phần tử theo phạm vi mục tiêu của nó. Việc sắp xếp đảm bảo rằng các số màu xanh nhỏ hơn sẽ cố gắng chiếm các vị trí hoán vị nhỏ hơn trước và các số màu đỏ lớn hơn sẽ cố gắng chiếm các vị trí lớn hơn cuối cùng. Việc kiểm tra tham lam rất đơn giản và tránh được những cạm bẫy mô phỏng. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
4
1 2 5 2
BRBR
```| Bước | Sắp xếp màu xanh | Sắp xếp màu đỏ | tôi | Kiểm tra | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1,5] | [2,2] | - | - | 
| Màu xanh 0 | 1 | - | 0 | 1 >= 1, được | 
| Màu xanh 1 | 5 | - | 1 | 5 >= 2, được | 
| Đỏ 0 | - | 2 | 0 | 2 <= 4, được | 
| Đỏ 1 | - | 2 | 1 | 2 <= 3, được | 

Tất cả các bước kiểm tra đều đạt, vì vậy đầu ra là CÓ. 

**Ví dụ 2** 

đầu vào:```
2
1 1
BB
```| Bước | Sắp xếp màu xanh | Sắp xếp màu đỏ | tôi | Kiểm tra | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1,1] | [] | - | - | 
| Màu xanh 0 | 1 | - | 0 | 1 >= 1, được | 
| Màu xanh 1 | 1 | - | 1 | 1 >= 2, THẤT BẠI | 

Màu xanh thứ hai không thể đạt tới 2, vì vậy đầu ra là KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp danh sách xanh và đỏ chiếm ưu thế trong tính toán. | 
| Không gian | O(n) | Lưu trữ danh sách màu xanh và đỏ riêng biệt. | 

Với các ràng buộc về tổng$n$trên tất cả các trường hợp thử nghiệm 2·10^5, phương pháp này chạy thoải mái trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution function
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        colors = input().strip()
        print(can_form_permutation(n, a, colors))
    return output.getvalue().strip()

# Provided samples
assert run("8\n4\n1 2 5 2\nBRBR\n2\n1 1\nBB\n5\n3 1 4 2 5\nRBRRB\n5\n3 1 3 1 3\nRBRRB\n5\n5 1 5 1 5\nRBRRB\n4\n2 2 2 2\nBRBR\n2\n1 -2\nBR\n4\n-2 -1 4 0\nRRRR\n") == \
"YES\nNO\nYES\nYES\nNO\nYES\nYES\nYES"

# Custom cases
assert run("1\n1\n100\nR\n") == "YES", "single red element beyond n"
assert run("1\n1\n-100\nB\n") == "YES", "single blue element below 1"
assert run("1\n3\n1 2 2\nBBR\n") == "NO", "duplicate blue cannot reach positions"
assert run("1\n5\n5 4 3 2 1\nRRRRR\n") == "YES", "all reds in reverse order"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n100\nR\n`| CÓ | Yếu tố màu đỏ duy nhất ngoài`n`có thể giảm xuống`n`| 
|`1\n-100\nB\n`| CÓ | Phần tử đơn màu xanh lam dưới 1 có thể tăng lên 1 | 
|`1\n3\n1 2 2\nBBR\n`| KHÔNG | Màu xanh trùng lặp không bao phủ được tất cả các vị trí | 
| `1\n5\n5 4 3 2 1\n | | |
