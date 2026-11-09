---
title: "CF 1475F - Ma trận bất thường"
description: "Chúng ta có hai ma trận nhị phân vuông có cùng kích thước, gọi là a và b. Mỗi phần tử là 0 hoặc 1. Các thao tác được phép là lật toàn bộ một hàng hoặc lật toàn bộ cột, trong đó lật có nghĩa là XOR từng phần tử bằng 1."
date: "2026-06-11T00:10:34+07:00"
tags: ["codeforces", "competitive-programming", "2-sat", "brute-force", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1475
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 697 (Div. 3)"
rating: 1900
weight: 1475
solve_time_s: 123
verified: true
draft: false
---

[CF 1475F - Ma trận bất thường](https://codeforces.com/problemset/problem/1475/F) 

**Xếp hạng:** 1900 
**Tags:** 2-sat, vũ phu, thuật toán xây dựng 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho ta hai ma trận nhị phân vuông có cùng kích thước, gọi là`a`Và`b`. Mỗi phần tử là 0 hoặc 1. Các thao tác được phép là lật toàn bộ một hàng hoặc lật toàn bộ cột, trong đó lật có nghĩa là XOR từng phần tử bằng 1. Nhiệm vụ của chúng ta là xác định xem có thể chuyển đổi hay không`a`vào trong`b`sử dụng bất kỳ trình tự nào của các lần lật hàng và cột này. 

Đầu vào chỉ định nhiều trường hợp thử nghiệm, mỗi trường hợp đưa ra kích thước của ma trận và chính hai ma trận đó. Đầu ra phải là "CÓ" nếu tồn tại một chuỗi các lần lật để làm cho ma trận bằng nhau hoặc "KHÔNG" nếu ngược lại. 

Ràng buộc chính thúc đẩy việc thiết kế thuật toán là`n`có thể lên tới 1000 và tổng số tiền`n`trên tất cả các trường hợp thử nghiệm không vượt quá 1000. Điều này ngụ ý rằng giải pháp bạo lực O(n^3) sẽ quá chậm, vì 1000^3 = 10^9 hoạt động. Cách tiếp cận O(n^2) khả thi vì chúng tôi được phép xử lý ma trận có tổng kích thước lên tới khoảng 10^6 phần tử trong vài giây. 

Trường hợp cạnh tinh tế là khi ma trận có kích thước 1x1 hoặc 2x2. Ví dụ: ma trận một phần tử chỉ có thể được lật một lần trong hàng và một lần trong cột, vì vậy chúng ta phải đảm bảo rằng giải pháp của chúng ta tính chẵn lẻ một cách chính xác. Một trường hợp phức tạp khác là khi các hàng và cột cần được lật xen kẽ; một cách tiếp cận ngây thơ, lật đổ mọi thứ một cách tham lam có thể tạo ra trạng thái trung gian bất khả thi và kết luận sai "KHÔNG". 

## Phương pháp tiếp cận 

Phương pháp đơn giản nhất là thử tất cả các chuỗi lật hàng và cột có thể có, nhưng có 2^n lần lật có thể xảy ra đối với hàng và 2^n đối với cột. Việc thử tất cả các kết hợp sẽ dẫn đến các hoạt động O(2^(2n) * n^2), tốc độ này chậm theo cấp số nhân đối với n ≥ 20. Lực lượng vũ phu này hoạt động trên lý thuyết vì mỗi lần lật hoặc thay đổi một ô hoặc không, vì vậy mọi cấu hình đều có thể truy cập được, nhưng trên thực tế, nó không thành công đối với các ma trận lớn. 

Thông tin chi tiết quan trọng là cần lưu ý rằng có thể nắm bắt được tác động của việc lật hàng và lật cột bằng cách xem xét sự khác biệt giữa`a`Và`b`. Đối với một ô cụ thể (i, j), sau vài lần lật, giá trị cuối cùng của nó sẽ là`a[i][j] XOR row_flip[i] XOR col_flip[j]`. Phương trình này phải bằng`b[i][j]`. Tương tự với mỗi ô ta có:```
row_flip[i] XOR col_flip[j] = a[i][j] XOR b[i][j]
```Nếu ta chọn các lần lật ở hàng đầu tiên một cách tùy ý thì các lần lật ở các cột được xác định duy nhất. Sau đó, các lần lật cho các hàng còn lại phải thỏa mãn các phương trình XOR tương tự; nếu họ làm vậy thì sự chuyển đổi là có thể. Điều này làm giảm vấn đề kiểm tra tính nhất quán của hệ phương trình XOR, có thể được thực hiện trong O(n^2). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(2n) * n^2) | O(n^2) | Quá chậm | 
| XOR chẵn lẻ/xây dựng | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính ma trận mới`d`Ở đâu`d[i][j] = a[i][j] XOR b[i][j]`. Mỗi mục thể hiện liệu`a[i][j]`cần phải được lật một số lần lẻ để khớp`b[i][j]`. Điều này chuyển đổi vấn đề từ các phép toán ma trận sang hệ thống các ràng buộc XOR. 
2. Quyết định tùy ý khi lật hàng đầu tiên, chẳng hạn, giả sử`row_flip[0] = 0`. Sau đó, với mỗi cột`j`, việc lật cột yêu cầu có thể được tính là`col_flip[j] = d[0][j] XOR row_flip[0]`. 
3. Đối với mỗi hàng còn lại`i > 0`, tính toán lật hàng cần thiết bằng cách so sánh cột đầu tiên:`row_flip[i] = d[i][0] XOR col_flip[0]`. Điều này đảm bảo rằng cột đầu tiên trong mỗi hàng khớp với mục tiêu. 
4. Xác minh rằng tất cả các mục khác thỏa mãn điều kiện XOR:`row_flip[i] XOR col_flip[j] == d[i][j]`. Nếu bất kỳ ô nào vi phạm phương trình này, hệ thống sẽ không nhất quán và câu trả lời là "KHÔNG". 
5. Nếu tất cả các ô đều thỏa mãn phương trình thì tồn tại chuỗi lần lật và câu trả lời là "CÓ". 

Tại sao nó hoạt động: Sau khi các lần lật cho hàng đầu tiên và cột đầu tiên được chọn, các lần lật còn lại sẽ được xác định. Hệ thống XOR đảm bảo rằng mọi ô đều nhất quán hoặc không. Cách tiếp cận này kiểm tra tất cả các ràng buộc về tính nhất quán mà không cần phải thử tất cả các chuỗi lần lật một cách rõ ràng. Sự lựa chọn tùy tiện của`row_flip[0]`có tác dụng vì việc lật hàng đầu tiên luôn có thể kết hợp với việc lật cột; nếu một giải pháp tồn tại, nó sẽ được phát hiện theo một trong hai lựa chọn`row_flip[0]`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = [list(map(int, list(input().strip()))) for _ in range(n)]
        input()  # skip empty line
        b = [list(map(int, list(input().strip()))) for _ in range(n)]
        
        # compute difference matrix
        d = [[a[i][j] ^ b[i][j] for j in range(n)] for i in range(n)]
        
        # assume first row flip = 0
        row_flip = [0] * n
        col_flip = [d[0][j] for j in range(n)]
        
        possible = True
        for i in range(1, n):
            row_flip[i] = d[i][0] ^ col_flip[0]
            for j in range(1, n):
                if row_flip[i] ^ col_flip[j] != d[i][j]:
                    possible = False
                    break
            if not possible:
                break
        print("YES" if possible else "NO")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ đọc các ma trận, tính toán chênh lệch XOR của chúng và khởi tạo các mảng lật hàng và cột. Nó đặt hàng đầu tiên về 0 và dẫn đến việc lật cột trực tiếp. Các hàng còn lại sau đó được kiểm tra theo các ràng buộc XOR. Nếu phát hiện thấy có sự mâu thuẫn, câu trả lời là "KHÔNG"; nếu không thì là "CÓ". Bỏ qua dòng trống giữa các ma trận sẽ tránh được sự sai lệch khi đọc đầu vào. Việc sử dụng khả năng hiểu danh sách đảm bảo rằng sự khác biệt của ma trận được tính toán một cách hiệu quả. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
3
3
110
001
110

000
000
000
```| Bước | d[i][j] | hàng_flip | col_flip | Kiểm tra | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 110 ^ 000 = 110 | row_flip[0]=0 | col_flip=[1,1,0] | - | 
| Hàng 1 | d[1][0]=0, col_flip[0]=1 => row_flip[1]=1 | row_flip=[0,1,?] | col_flip=[1,1,0] | kiểm tra d[1][1]=0? row_flip^col_flip=1^1=0 , d[1][2]=1? row_flip^col_flip=1^0=1 | 
| Hàng 2 | d[2][0]=1, col_flip[0]=1 => row_flip[2]=0 | row_flip=[0,1,0] | col_flip=[1,1,0] | kiểm tra d[2][1]=1? 0^1=1 , d[2][2]=0? 0^0=0 | 

Tất cả các ô đều thỏa mãn điều kiện XOR, xuất ra "CÓ". 

**Ví dụ 2** 

đầu vào:```
2
01
11

10
10
```| Bước | d[i][j] | hàng_flip | col_flip | Kiểm tra | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 01^10=11, 11^10=01 | row_flip[0]=0 | col_flip=[1,1] | Kiểm tra hàng 1: row_flip[1]=0^1=1, kiểm tra d[1][1]=1^1=0? d[1][1]=1 | 

Phát hiện sự không nhất quán, xuất ra "NO". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi trường hợp thử nghiệm yêu cầu tính toán ma trận sai phân và xác minh từng ô một lần. | 
| Không gian | O(n^2) | Lưu trữ`a`,`b`, Và`d`ma trận. Lật hàng và lật cột sử dụng không gian bổ sung O(n). | 

Với tổng n trên tất cả các trường hợp thử nghiệm không vượt quá 1000, tổng số thao tác dưới 10^6, trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("""3
3
110
001
110

000
000
000
3
101
010
101

010
101
010
2
01
11

10
10""") == "YES\nYES\nNO"

# Custom tests
# Minimum-size input
assert run("""1
1
0

1""") == "YES", "1x1, flip required"
assert run("""1
1
1

1""") == "YES", "1x1, no flip required"

#
```
