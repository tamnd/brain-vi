---
title: "CF 1797B - Li Hua và Hoa văn"
description: "Chúng ta có một lưới $n lần n$ trong đó mỗi ô là 0 hoặc 1, đại diện cho hai màu. Chúng ta cũng được cho một số lần di chuyển cố định $k$. Một lần di chuyển sẽ lật màu của chính xác một ô đã chọn."
date: "2026-06-09T09:56:12+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1797
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 864 (Div. 2)"
rating: 1100
weight: 1797
solve_time_s: 81
verified: true
draft: false
---

[CF 1797B - Li Hua và Pattern](https://codeforces.com/problemset/problem/1797/B) 

**Đánh giá:** 1100 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$lưới trong đó mỗi ô là 0 hoặc 1, biểu thị hai màu. Chúng tôi cũng được cấp một số cố định$k$của các bước di chuyển. Một lần di chuyển sẽ lật màu của chính xác một ô đã chọn. Chúng ta có thể chọn cùng một ô nhiều lần nên mỗi lần di chuyển đều độc lập và chỉ chuyển đổi một chút. 

Mục tiêu không phải là đạt được một cấu hình tùy ý mà là kết thúc ở một điều kiện cấu trúc rất cụ thể: lưới phải trở nên giống hệt với phiên bản của nó được xoay bởi$180^\circ$. Điều này có nghĩa là với mỗi ô$(i, j)$, giá trị của nó phải khớp với ô$(n-1-i, n-1-j)$. 

Khó khăn chính là chúng ta phải thực hiện chính xác$k$lật, không phải “nhiều nhất$k$Những lần lật bổ sung không thể bị loại bỏ nên nếu chúng ta sửa lỗi đối xứng sớm thì các thao tác còn lại vẫn phải sử dụng được mà không vi phạm điều kiện. 

Các ràng buộc gợi ý quét tuyến tính cho mỗi trường hợp thử nghiệm là đủ. Tổng cộng$n$trong tất cả các bài kiểm tra nhiều nhất là$10^3$, vì vậy một$O(n^2)$giải pháp dễ dàng đủ nhanh. Bất cứ điều gì liên quan đến việc mô phỏng lặp đi lặp lại các hoạt động hoặc tìm kiếm trên các tập hợp con đều không cần thiết. 

Trường hợp cạnh tinh tế xuất hiện khi lưới đã đối xứng. Trong trường hợp đó, câu trả lời phụ thuộc hoàn toàn vào việc liệu chúng ta có thể lãng phí chính xác$k$hoạt động trong khi vẫn bảo toàn tính đối xứng. Một trường hợp cạnh khác phát sinh khi$n$là số lẻ và ô trung tâm tồn tại một mình khi quay; việc lật nó chỉ ảnh hưởng đến chính nó, điều này làm thay đổi các ràng buộc chẵn lẻ đối với các nước đi có thể sử dụng được. 

## Phương pháp tiếp cận 

Một ý tưởng bạo lực sẽ cố gắng mô phỏng tất cả các chuỗi của$k$lật và kiểm tra xem có chuỗi nào dẫn đến lưới đối xứng hay không. Điều này rõ ràng là không khả thi vì mỗi lần di chuyển sẽ nhân đôi không gian trạng thái và bản thân lưới đã có sẵn$2^{n^2}$tiểu bang. Ngay cả một tìm kiếm hạn chế hơn chỉ xem xét các tập hợp con của các ô bị lật cũng dẫn đến sự bùng nổ tổ hợp. 

Quan sát quan trọng là tính đối xứng dưới$180^\circ$phân chia lưới thành các cặp ô rời rạc$(i, j)$Và$(n-1-i, n-1-j)$, ngoại trừ có thể là ô trung tâm khi$n$thật kỳ quặc. Mỗi cặp phải kết thúc bằng nhau. Đối với một cặp không khớp, cần chính xác một lần lật để sửa chúng, vì việc lật một trong hai ô sẽ khiến chúng khớp nhau. Do đó, mỗi cặp đóng góp một chi phí bắt buộc là 0 hoặc 1 thao tác tùy thuộc vào việc nó có bằng nhau hay không. 

Cho phép$d$là số cặp đối xứng không khớp. Chúng ta phải dành ít nhất$d$hoạt động để sửa tất cả các cặp. Sau khi sửa tính đối xứng, mọi thao tác tiếp theo đều phải bảo toàn tính đối xứng. Trạng thái đối xứng cho phép hai loại thao tác trung lập: lật cả hai ô của một cặp hai lần hoặc lật bất kỳ cặp nào với số lần chẵn và đối với ô trung tâm (nếu nó tồn tại), mỗi lần lật sẽ chuyển đổi trực tiếp tính đối xứng. 

Điều này làm giảm vấn đề thành câu hỏi chẵn lẻ: sau khi đạt được cấu hình đối xứng, chúng ta phải kiểm tra xem liệu chúng ta có thể phân phối phần còn lại hay không$k - d$hoạt động mà không phá vỡ tính đối xứng. Mỗi thao tác đối xứng thực tế tốn 2 lần lật cho các cặp, trong khi ô trung tâm tốn 1 lần lật cho mỗi lần chuyển đổi. Do đó, khả năng hấp thụ các bước di chuyển bổ sung phụ thuộc vào việc chúng ta có ít nhất một “kênh chẵn lẻ tự do” hay không. 

Nếu như$n$là số lẻ, ô trung tâm đưa ra điều chỉnh chẵn lẻ trực tiếp, do đó, bất kỳ số bước di chuyển còn lại nào cũng có thể sử dụng được. Nếu như$n$chẵn, các phép toán phải đi theo cặp nên các nước đi còn lại phải chẵn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | hàm mũ | Quá chậm | 
| Tối ưu |$O(n^2)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Ghép từng ô$(i, j)$với$(n-1-i, n-1-j)$. Đếm xem có bao nhiêu cặp khác nhau. Mỗi cặp khác nhau đại diện cho một sự điều chỉnh bắt buộc tiêu tốn một lần lật. 
2. Hãy tính số này$d$. Nếu như$d > k$, chúng tôi đã thất bại vì ngay cả số lần lật tối thiểu được yêu cầu cũng vượt quá ngân sách. 
3. Tính các phép toán còn lại$r = k - d$. Đây là những bước di chuyển phải được thực hiện mà không phá vỡ tính đối xứng cuối cùng. 
4. Nếu$n$chẵn, kiểm tra xem$r$là chẵn. Nếu là số lẻ, chúng ta không thể phân phối các lần lật theo cách bảo toàn đối xứng, vì vậy câu trả lời là KHÔNG. 
5. Nếu$n$là số lẻ, ô trung tâm cho phép hấp thụ bất kỳ tính chẵn lẻ nào của các bước di chuyển còn lại, do đó, bất kỳ$r \ge 0$hoạt động. 

Tại sao nó hoạt động 

Mỗi cặp không khớp có chính xác hai ô được liên kết bằng cách xoay. Việc sửa đối xứng sẽ giảm độc lập trên mỗi cặp vì việc lật một ô chỉ thay đổi trạng thái không khớp của cặp đó. Khi tất cả các cặp khớp nhau, cấu hình sẽ được đóng lại theo vòng quay. Từ thời điểm đó, các phép toán hợp lệ phải bảo toàn tính đối xứng, điều này buộc các lần lật xảy ra theo mô hình đối xứng. Thậm chí$n$, tất cả các phép toán hợp lệ đều được ghép nối, do đó tổng số lần lật bổ sung phải là số chẵn. Đối với số lẻ$n$, ô trung tâm hoạt động như một phần chìm chẵn lẻ, cho phép đếm mọi thao tác còn sót lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]
    
    mismatches = 0
    
    for i in range(n):
        for j in range(n):
            ni, nj = n - 1 - i, n - 1 - j
            if (i, j) < (ni, nj):
                if a[i][j] != a[ni][nj]:
                    mismatches += 1
    
    if mismatches > k:
        print("NO")
        return
    
    rem = k - mismatches
    
    if n % 2 == 0:
        print("YES" if rem % 2 == 0 else "NO")
    else:
        print("YES")

t = int(input())
for _ in range(t):
    solve()
```Lưới được quét một lần và mỗi cặp đối xứng được tính chính xác một lần bằng cách thực thi thứ tự giữa$(i, j)$và tấm gương của nó$(ni, nj)$. Điều này tránh việc tính hai lần và tránh việc đặt trung tâm vào vị trí đặc biệt trong quá trình tính toán không khớp. 

Logic quyết định sau đó tách thành hai trường hợp dựa trên tính chẵn lẻ của$n$. Trường hợp chẵn bắt buộc rằng các bước di chuyển còn sót lại phải được ghép nối thành các hoạt động bảo toàn đối xứng. Trường hợp lẻ bỏ qua tính chẵn lẻ vì ô trung tâm cung cấp một nút chuyển đổi một ô giúp hấp thụ mọi nước đi lẻ còn sót lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
1 0 1 1
1 0 0 0
0 1 0 1
1 1 0 1
```Chúng tôi tính toán các cặp không khớp: 

| Cặp | Giá trị | Trận đấu | Không khớp | 
| --- | --- | --- | --- | 
| (0,0)-(3,3) | 1 đấu 1 | vâng | 0 | 
| (0,1)-(3,2) | 0 vs 0 | vâng | 0 | 
| (0,2)-(3,1) | 1 đấu 1 | vâng | 0 | 
| (0,3)-(3,0) | 1 đấu 1 | vâng | 0 | 
| (1,0)-(2,3) | 1 đấu 1 | vâng | 0 | 
| (1,1)-(2,2) | 0 vs 0 | vâng | 0 | 
| (1,2)-(2,1) | 0 vs 1 | không | 1 | 
| (1,3)-(2,0) | 0 vs 0 | vâng | 0 | 

Vì thế$d = 1$. Còn lại$r = 2$. Từ$n=4$là chẵn,$r$phải chẵn, và nó chẵn, nên câu trả lời là CÓ. 

Dấu vết này cho thấy chỉ có một cặp thực hiện hiệu chỉnh và các thao tác còn sót lại có thể được ghép nối mà không phá vỡ tính đối xứng. 

### Ví dụ 2 

Hãy xem xét:```
3 4
0 0 0
0 1 0
0 0 0
```Ở đây cấu trúc trung tâm duy nhất là ô ở giữa, nó ánh xạ tới chính nó. 

Không khớp: 

| Cặp | Giá trị | Không khớp | 
| --- | --- | --- | 
| góc | tất cả 0 | 0 | 
| cạnh | tất cả 0 | 0 | 
| trung tâm | 1 | xử lý riêng | 

Vì thế$d = 0$,$r = 4$. Từ$n=3$là số lẻ, mọi số dư đều được chấp nhận. 

Điều này thể hiện sự khác biệt chính giữa kích thước chẵn và kích thước lẻ: lưới lẻ có bộ đệm chẵn lẻ ở giữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| mỗi ô được truy cập một lần để tạo thành các cặp đối xứng | 
| Không gian |$O(1)$| chỉ sử dụng bộ đếm và bộ nhớ đầu vào | 

Tổng số tiền của$n$trên nhiều trường hợp thử nghiệm là nhiều nhất$10^3$, do đó việc quét tất cả các lưới vẫn nằm trong giới hạn một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n, k = map(int, input().split())
        a = [list(map(int, input().split())) for _ in range(n)]
        mismatches = 0
        for i in range(n):
            for j in range(n):
                ni, nj = n - 1 - i, n - 1 - j
                if (i, j) < (ni, nj):
                    if a[i][j] != a[ni][nj]:
                        mismatches += 1
        if mismatches > k:
            return "NO"
        rem = k - mismatches
        if n % 2 == 0:
            return "YES" if rem % 2 == 0 else "NO"
        return "YES"

    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve())
    return "\n".join(out)

# provided samples
assert run("""3
4 0
1 1 1 1
0 0 0 1
1 0 1 0
1 1 1 1
4 3
1 0 1 1
1 0 0 0
0 1 0 1
1 1 0 1
5 4
0 0 0 0 0
0 1 1 1 1
0 1 0 0 0
1 1 1 1 1
0 0 0 0 0
""") == """NO
YES
YES"""

# custom cases
assert run("""1
1 1
1
""") == "YES"

assert run("""1
2 1
1 0
0 1
""") == "NO"

assert run("""1
2 2
1 0
0 1
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 k lẻ | CÓ | lưới lẻ hấp thụ tính chẵn lẻ | 
| 2x2 một lần lật không thể | KHÔNG | chẵn lẻ không khớp trong lưới chẵn | 
| Có thể thực hiện 2x2 hai lần lật | CÓ | ngay cả những tác phẩm còn sót lại | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi lưới đã đối xứng nhưng$k$là nhỏ và sự đồng đều là quan trọng. Ví dụ:```
n = 2, k = 1
1 0
0 1
```Không có sự không phù hợp nào, nhưng$k=1$. Từ$n$chẵn, số chẵn lẻ còn lại là số lẻ và không thể được hấp thụ một cách đối xứng. Thuật toán xuất ra NO một cách chính xác vì$r=1$thật kỳ quặc. 

Đối với số lẻ$n$, coi như:```
n = 3, k = 1
0 0 0
0 1 0
0 0 0
```Không có sự không khớp nào tồn tại, nhưng vẫn còn một thao tác. Ô trung tâm có thể được lật, bảo toàn các ràng buộc đối xứng quay xét về tính khả thi của trạng thái cuối cùng vì nó tự chuyển đổi mà không ảnh hưởng đến các cặp. Thuật toán đưa ra CÓ, phản ánh rằng tính chẵn lẻ còn sót lại luôn có thể được hấp thụ ở các chiều lẻ.
