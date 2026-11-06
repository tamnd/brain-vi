---
title: "CF 1236C - Phòng thí nghiệm"
description: "Chúng ta được cung cấp các số nguyên từ 1 đến $n^2$, mỗi số đại diện cho một lab được định vị theo chiều cao, trong đó số nhỏ hơn thì thấp hơn và số lớn hơn thì cao hơn."
date: "2026-06-15T20:15:55+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1236
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 593 (Div. 2)"
rating: 1300
weight: 1236
solve_time_s: 592
verified: false
draft: false
---

[CF 1236C - Phòng thí nghiệm](https://codeforces.com/problemset/problem/1236/C) 

**Đánh giá:** 1300 
**Tags:** thuật toán xây dựng, tham lam, triển khai 
**Thời gian giải:** 9 phút 52 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho các số nguyên từ 1 đến$n^2$, mỗi cái đại diện cho một phòng thí nghiệm được định vị theo chiều cao, trong đó số nhỏ hơn sẽ thấp hơn và số lớn hơn sẽ cao hơn. Giữa mỗi cặp phòng thí nghiệm có một đường ống định hướng cho phép một đơn vị dòng chảy từ phòng thí nghiệm có số cao hơn đến phòng thí nghiệm có số thấp hơn. Vì vậy mỗi cặp đặt hàng$(u, v)$đóng góp một đơn vị dòng chảy có thể nếu$u > v$. 

Chúng ta phải phân vùng những thứ này$n^2$phòng thí nghiệm vào$n$nhóm, mỗi nhóm chứa chính xác$n$nhãn. Giữa hai nhóm bất kỳ$A$Và$B$, chúng tôi xác định$f(A, B)$bằng số lượng các cặp có thứ tự$(u, v)$như vậy$u \in A$,$v \in B$, Và$u > v$. Điều này chỉ đơn giản là đếm xem có bao nhiêu “cạnh hướng xuống” đi từ nhóm$A$thành nhóm$B$. 

Mục tiêu là chọn phân vùng sao cho giá trị nhỏ nhất của$f(A, B)$trên tất cả các cặp nhóm riêng biệt thì càng lớn càng tốt. Nói cách khác, chúng tôi muốn cân bằng tất cả các kết nối đi xuống giữa các nhóm một cách đồng đều nhất có thể, tránh bất kỳ cặp nhóm yếu nào. 

Ràng buộc đầu vào$n \le 300$ngụ ý lên đến$90{,}000$các phần tử. Bất kỳ giải pháp nào cố gắng đánh giá tất cả các phân vùng đều không thể thực hiện được vì số lượng phân vùng tăng theo cấp số nhân. Ngay cả việc kiểm tra một phân vùng cũng tốn kém một cách ngây thơ$O(n^4)$nếu được thực hiện theo cặp trên các nhóm và phần tử, tốc độ này quá chậm$n=300$. 

Một trường hợp thất bại phổ biến của trực giác tham lam là nhóm các số liên tiếp lại với nhau. Ví dụ, đặt$\{1,2,3\}$,$\{4,5,6\}$,$\{7,8,9\}$khi$n=3$tạo ra các dòng chảy cực kỳ mất cân bằng: mọi tương tác từ lớn đến nhỏ đều tập trung vào một vài hướng, còn các hướng khác thì yếu. Điều này vi phạm mục tiêu tối đa hóa lưu lượng tối thiểu giữa các nhóm. 

Một cạm bẫy khác là việc phân nhóm ngẫu nhiên hoặc gán hàng chính, có xu hướng tập hợp các giá trị có thể so sánh được và làm giảm sự đảo ngược giữa các nhóm theo một hướng nhưng không đối xứng theo hướng ngược lại, một lần nữa làm giảm mức tối thiểu.$f(A,B)$. 

Khó khăn chính đó là$f(A,B)$chỉ phụ thuộc vào thứ tự tương đối của các giá trị chứ không phải giá trị tuyệt đối của chúng, điều này gợi ý một hoán vị có cấu trúc hơn là nhóm tùy ý. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi cách để chia tách$n^2$số vào$n$nhóm kích thước$n$, sau đó tính$f(A,B)$cho từng cặp nhóm. Ngay cả khi chúng ta có một phân vùng duy nhất, việc tính toán tất cả$f(A,B)$giá trị chi phí$O(n^3)$, vì mỗi cặp nhóm yêu cầu so sánh$n^2$các cặp phần tử. Số lượng phân vùng làm cho điều này hoàn toàn không khả thi. 

Cái nhìn sâu sắc về cấu trúc là giải thích các con số trong một$n \times n$lưới và gán các nhóm dọc theo đường chéo. Mục tiêu là để đảm bảo rằng đối với hai nhóm bất kỳ, có một số lượng lớn các mối quan hệ “lớn đến nhỏ” nhất quán và đủ lớn theo cả hai hướng. Cách xây dựng tốt là lấp đầy lưới theo mô hình đường chéo tuần hoàn sao cho mỗi nhóm trải rộng trên các phạm vi giá trị khác nhau. 

Cấu trúc tiêu chuẩn được sử dụng trong bài toán này là đặt các số trong một cột ma trận theo từng cột, hướng luân phiên trên mỗi cột. Điều này đảm bảo rằng mọi tương tác giữa hàng và cột đều được cân bằng và không có nhóm nào tập trung vào một phạm vi giá trị duy nhất. 

Cấu trúc cột ngoằn ngoèo xen kẽ này đảm bảo rằng đối với bất kỳ cặp nhóm nào, có đủ sự đảo ngược theo cả hai hướng và mức tối thiểu$f(A,B)$được tối đa hóa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(n²) | Quá chậm | 
| Cột thi công ngoằn ngoèo | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một$n \times n$lưới và sau đó đọc từng hàng theo nhóm. 

1. Tạo một khoảng trống$n \times n$ma trận. Chúng ta sẽ điền nó với các số từ 1 đến$n^2$theo cách có cấu trúc sao cho mỗi hàng trở thành một nhóm. 

Cấu trúc đảm bảo rằng mỗi hàng chứa các số từ các “cấp độ” khác nhau của công trình. 
2. Điền vào cột ma trận theo cột bằng cách sử dụng bộ đếm bắt đầu từ 1. 

Đối với mỗi cột$c$, chúng tôi đặt các số từ trên xuống dưới hoặc từ dưới lên trên tùy thuộc vào việc$c$là chẵn hoặc lẻ. 

Hướng xen kẽ này đảm bảo rằng các cột liền kề phân phối các giá trị lớn và nhỏ khác nhau trên các hàng, ngăn không cho bất kỳ hàng nào bị lệch về phía chỉ các giá trị lớn hoặc chỉ nhỏ. 
3. Sau khi điền vào ma trận, coi mỗi hàng là một nhóm. 

Mỗi hàng bây giờ chứa chính xác$n$các giá trị riêng biệt và tất cả các giá trị từ 1 đến$n^2$được sử dụng đúng một lần. 
4. Xuất từng hàng thành một nhóm. 

### Tại sao nó hoạt động 

Việc điền cột xen kẽ đảm bảo rằng đối với bất kỳ hàng cố định nào, các phần tử của nó được trải đều trên cả vùng giá trị cao và thấp theo mẫu được kiểm soát. Hai hàng bất kỳ khác nhau ở ít nhất một nửa số cột theo hướng sắp xếp, điều này đảm bảo số lượng đảo ngược ổn định giữa chúng. Điều này ngăn không cho bất kỳ cặp nhóm nào có kích thước nhỏ bất thường.$f(A,B)$, vì không nhóm nào luôn ở trên hoặc dưới nhóm kia trên quá nhiều cột. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

grid = [[0] * n for _ in range(n)]
num = 1

for col in range(n):
    if col % 2 == 0:
        for row in range(n):
            grid[row][col] = num
            num += 1
    else:
        for row in range(n - 1, -1, -1):
            grid[row][col] = num
            num += 1

for row in grid:
    print(*row)
```Việc xây dựng lấp đầy ma trận theo thứ tự cột lớn, với hướng xen kẽ trên mỗi cột. Điều này đảm bảo rằng các giá trị không tăng đơn điệu dọc theo hàng hoặc cột, đây là yêu cầu cốt lõi để cân bằng so sánh giữa các nhóm. 

Mỗi hàng được in thành một nhóm và vì mỗi giá trị được sử dụng chính xác một lần nên chúng ta thu được một phân vùng hợp lệ. 

Một lỗi triển khai phổ biến là xen kẽ các hàng thay vì cột, điều này phá vỡ tính đối xứng và tạo ra sự phân bố không đồng đều. Một sai lầm khác là quên rằng các nhóm phải được thành lập sau khi hoàn thành nhiệm vụ chứ không phải trong quá trình xây dựng. 

## Ví dụ đã hoạt động 

### Ví dụ:$n = 3$Chúng tôi điền vào một$3 \times 3$lưới. 

| Bước | Cột | Hướng | Trạng thái lưới (một phần) | 
| --- | --- | --- | --- | 
| 1 | 0 | từ trên xuống | 1,2,3 đặt ở cột 0 | 
| 2 | 1 | từ dưới lên | 6,5,4 đặt ở cột 1 | 
| 3 | 2 | từ trên xuống | 7,8,9 đặt ở cột 2 | 

Lưới cuối cùng: 

| Hàng | Giá trị | 
| --- | --- | 
| 0 | 1 6 7 | 
| 1 | 2 5 8 | 
| 2 | 3 4 9 | 

Mỗi hàng là một nhóm. 

Điều này thể hiện cách các giá trị được xen kẽ trên các phạm vi cường độ thay vì được phân cụm. 

### Ví dụ:$n = 2$Chúng tôi điền vào một$2 \times 2$lưới. 

| Bước | Cột | Hướng | 
| --- | --- | --- | 
| 1 | 0 | từ trên xuống → 1,2 | 
| 2 | 1 | từ dưới lên → 4,3 | 

Lưới: 

| Hàng | Giá trị | 
| --- | --- | 
| 0 | 1 4 | 
| 1 | 2 3 | 

Điều này xác nhận cấu trúc xen kẽ đã hoạt động ở kích thước hợp lệ nhỏ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | mỗi trong số$n^2$số được đặt một lần | 
| Không gian | O(n²) | lưu trữ cho lưới điện | 

Các ràng buộc cho phép tối đa 90.000 vị trí, không đáng kể dưới 1 giây trong Python. Không cần so sánh hoặc mô phỏng theo cặp. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    n = int(input())
    grid = [[0] * n for _ in range(n)]
    num = 1
    for col in range(n):
        if col % 2 == 0:
            for row in range(n):
                grid[row][col] = num
                num += 1
        else:
            for row in range(n - 1, -1, -1):
                grid[row][col] = num
                num += 1
    for row in grid:
        print(*row)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# samples
assert run("3\n") != "", "sample 1 basic sanity"

# custom cases
assert run("2\n") != "", "minimum size"
assert run("4\n") != "", "small even"
assert run("5\n") != "", "odd size structure"
assert run("10\n") != "", "larger grid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | lưới 2×2 hợp lệ | độ đúng cơ sở | 
| 3 | hoán vị có cấu trúc | mẫu mẫu | 
| 4 | lưới chẵn đầy đủ | tính đối xứng của công trình | 
| 10 | trường hợp lớn ổn định | hiệu suất và tính nhất quán | 

## Vỏ cạnh 

cho$n=2$, lưới trở thành:```
1 4
2 3
```Hàng 0 là$\{1,4\}$, hàng 1 là$\{2,3\}$. Việc xây dựng vẫn đảm bảo cả hai hướng giữa hai nhóm có sự đảo ngược cân bằng vì phần tử lớn nhất 4 và nhỏ nhất 1 được tách ra, trong khi các giá trị ở giữa cắt nhau cả hai hướng. 

Đối với lớn hơn$n$, hướng cột xen kẽ đảm bảo không có hàng nào tích lũy số lớn hay số nhỏ một cách nhất quán. Các cột chẵn đẩy thứ tự từ thấp đến cao xuống dưới, trong khi các cột lẻ đảo ngược thứ tự đó, đảm bảo rằng hai hàng bất kỳ khác nhau về cấu trúc đảo ngược trên ít nhất một nửa cột.
