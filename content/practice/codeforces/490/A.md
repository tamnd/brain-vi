---
title: "CF 490A - Olympic đồng đội"
description: "Chúng tôi được cung cấp một danh sách trẻ em, mỗi trẻ được xác định bằng một con số cho biết kỹ năng của chúng: lập trình (1), toán (2) hoặc thể dục (3). Nhiệm vụ là thành lập càng nhiều đội gồm ba người càng tốt, trong đó mỗi đội có chính xác một đứa trẻ từ mỗi kỹ năng."
date: "2026-06-07T17:40:01+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 490
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 279 (Div. 2)"
rating: 800
weight: 490
solve_time_s: 117
verified: false
draft: false
---

[CF 490A - Olympic đồng đội](https://codeforces.com/problemset/problem/490/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 1 phút 57 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách trẻ em, mỗi trẻ được xác định bằng một con số cho biết kỹ năng của chúng: lập trình (1), toán (2) hoặc thể dục (3). Nhiệm vụ là thành lập càng nhiều đội gồm ba người càng tốt, trong đó mỗi đội có chính xác một đứa trẻ từ mỗi kỹ năng. Đầu ra trước tiên phải nêu số lượng đội và sau đó liệt kê các chỉ số của trẻ em trong mỗi đội. Mỗi đứa trẻ chỉ có thể xuất hiện trong một đội. 

Hạn chế chính đó là$n \le 5000$. Kích thước này đủ nhỏ để cho phép quét dữ liệu tuyến tính hoặc gần tuyến tính và thậm chí các thao tác đơn giản trên tất cả trẻ em nhiều lần cũng sẽ hoạt động. Các chỉ mục phải được bảo toàn theo đầu vào ban đầu cho mục đích đầu ra nhưng chúng tôi có thể tự do sắp xếp lại hoặc lưu trữ chúng khi cần thiết trong nội bộ. 

Trường hợp khó khăn không rõ ràng xảy ra khi một kỹ năng không được thể hiện đúng mức. Ví dụ, với đầu vào`1 1 2 2 2 3`, chúng ta có ba lập trình viên, ba nhà toán học và một vận động viên. Số lượng đội tối đa rõ ràng là 1, vì chúng ta không thể thành lập nhiều hơn một đội với đúng một con của mỗi kỹ năng. Một cách tiếp cận ngây thơ cố gắng chỉ ghép trẻ em một cách tuần tự có thể tạo ra hai đội do nhầm lẫn và không đáp ứng được yêu cầu về kỹ năng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng thử tất cả các cách kết hợp có thể có của ba đứa trẻ và kiểm tra xem chúng có tạo thành một đội hợp lệ hay không. Với số lượng lên tới 5000 đứa trẻ, số lượng sinh ba có thể là$\binom{5000}{3}$, khoảng 20 tỷ, điều này rõ ràng là không khả thi. Về mặt lý thuyết, điều này có hiệu quả vì nó kiểm tra mọi kết hợp nhưng lại thất bại khi$n$phát triển vượt quá vài chục. 

Cái nhìn sâu sắc quan trọng là tách trẻ em theo kỹ năng. Khi chúng tôi có ba danh sách - một dành cho lập trình viên, một dành cho nhà toán học và một dành cho vận động viên thể thao - số lượng đội tối đa bị giới hạn bởi danh sách nhỏ nhất, bởi vì chúng tôi không thể thành lập một đội mà không có ít nhất một đứa trẻ từ mỗi kỹ năng. Sau đó, chúng tôi có thể chọn trẻ em ở đầu mỗi danh sách và thành lập các nhóm một cách tuần tự. Điều này đảm bảo rằng mỗi nhóm đều có những kỹ năng riêng biệt và chúng tôi đương nhiên tôn trọng quy định mỗi nhóm một trẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo ba danh sách trống:`prog`dành cho lập trình viên,`maths`dành cho các nhà toán học và`pe`dành cho vận động viên thể thao. Chúng sẽ lưu trữ các chỉ số của trẻ với từng kỹ năng. 
2. Lặp lại danh sách các phần tử con. Đối với mỗi đứa trẻ, hãy thêm chỉ số dựa trên 1 của nó vào danh sách thích hợp tùy thuộc vào kỹ năng. 
3. Tính độ dài tối thiểu giữa ba danh sách. Con số này,`w`, là số lượng đội tối đa có thể được thành lập. 
4. Với mỗi số nguyên`i`từ 0 đến`w-1`, xuất ra một đội bao gồm`i`-phần tử thứ từ`prog`,`maths`, Và`pe`. Điều này đảm bảo rằng mỗi đội có chính xác một con của mỗi kỹ năng và không có con nào bị lặp lại. 
5. In`w`đầu tiên, sau đó in từng đội ra một dòng riêng. 

Tại sao nó hiệu quả: Ở mỗi bước, chúng tôi chọn chính xác một trẻ từ mỗi kỹ năng. Bằng cách lặp lại số lượng tối thiểu trong số ba nhóm kỹ năng, chúng tôi đảm bảo rằng chúng tôi không bao giờ vượt quá số lượng trẻ có sẵn ở bất kỳ kỹ năng nào. Điều này tạo ra số lượng nhóm hợp lệ tối đa và vì chúng tôi theo dõi trực tiếp các chỉ số nên độ chính xác đầu ra được giữ nguyên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
skills = list(map(int, input().split()))

prog = []
maths = []
pe = []

for idx, s in enumerate(skills, 1):
    if s == 1:
        prog.append(idx)
    elif s == 2:
        maths.append(idx)
    else:
        pe.append(idx)

w = min(len(prog), len(maths), len(pe))
print(w)

for i in range(w):
    print(prog[i], maths[i], pe[i])
```Giải pháp đọc số lượng trẻ em và kỹ năng của chúng. Nó tách các chỉ số thành ba danh sách dựa trên kỹ năng. Sau đó, nó tính toán số lượng đội tối đa bằng độ dài tối thiểu của ba danh sách. Cuối cùng, nó in`w`và xuất ra mỗi đội bằng cách lấy một chỉ mục từ mỗi danh sách kỹ năng theo thứ tự. Sử dụng lập chỉ mục dựa trên 1 là rất quan trọng vì vấn đề yêu cầu nó. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
7
1 3 1 3 2 1 2
```| chuyên nghiệp | toán học | pe | w | Đội được thành lập | 
| --- | --- | --- | --- | --- | 
| [1,3,6] | [5,7] | [2,4] | phút(3,2,2)=2 | (1,5,2), (3,7,4) | 

Chúng tôi chọn hai đội, vì nhóm kỹ năng nhỏ nhất (`maths`Và`pe`) mỗi người có 2 người con. Bảng này cho thấy cách các chỉ số được chọn tuần tự từ mỗi danh sách. 

Đầu vào mẫu 2:```
4
1 1 2 3
```| chuyên nghiệp | toán học | pe | w | Đội được thành lập | 
| --- | --- | --- | --- | --- | 
| [1,2] | [3] | [4] | phút(2,1,1)=1 | (1,3,4) | 

Chỉ có thể thành lập một đội, bị giới hạn bởi số lượng nhà toán học và vận động viên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng ta lặp qua danh sách con một lần để phân tách các chỉ số, sau đó lặp lên đến`w`(≤ n) để thành lập đội. | 
| Không gian | O(n) | Chúng tôi lưu trữ các chỉ mục trong ba danh sách riêng biệt, mỗi danh sách có tổng cộng n phần tử. | 

Giải pháp dễ dàng phù hợp với giới hạn thời gian 1 giây cho n 5000 và giới hạn bộ nhớ là 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution
    n = int(input())
    skills = list(map(int, input().split()))
    prog, maths, pe = [], [], []
    for idx, s in enumerate(skills, 1):
        if s == 1:
            prog.append(idx)
        elif s == 2:
            maths.append(idx)
        else:
            pe.append(idx)
    w = min(len(prog), len(maths), len(pe))
    print(w)
    for i in range(w):
        print(prog[i], maths[i], pe[i])
    return output.getvalue().strip()

# provided sample
assert run("7\n1 3 1 3 2 1 2\n") == "2\n1 5 2\n3 7 4", "sample 1"

# custom cases
assert run("4\n1 1 2 3\n") == "1\n1 3 4", "minimum teams"
assert run("6\n1 1 1 2 2 3\n") == "1\n1 4 6", "one team only"
assert run("3\n1 2 3\n") == "1\n1 2 3", "exactly one team"
assert run("5\n1 2 3 1 2\n") == "1\n1 2 3", "extra children in skill groups"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4\n1 1 2 3 | 1\n1 3 4 | Chỉ có thể thành lập một đội | 
| 6\n1 1 1 2 2 3 | 1\n1 4 6 | Số lượng tối thiểu xác định đội | 
| 3\n1 2 3 | 1\n1 2 3 | Trường hợp cạnh đội đơn | 
| 5\n1 2 3 1 2 | 1\n1 2 3 | Trẻ bổ sung không tạo thêm đội | 

## Vỏ cạnh 

Ví dụ: nếu một kỹ năng không có con`3\n1 1 2`, thì danh sách trở thành`prog=[1,2]`,`maths=[3]`,`pe=[]`. Độ dài tối thiểu là 0, vì vậy không thể thành lập đội. Thuật toán in chính xác 0. Điều này xử lý trường hợp cạnh mà không cần kiểm tra thêm vì thao tác tối thiểu thực thi nó một cách tự nhiên.
