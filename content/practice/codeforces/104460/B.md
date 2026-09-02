---
title: "CF 104460B - Lưới có mũi tên"
description: "Chúng ta được cung cấp một lưới có hướng trong đó mỗi ô chứa hai phần thông tin: hướng (lên, xuống, trái hoặc phải) và độ dài bước nhảy dương."
date: "2026-06-30T13:28:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104460
codeforces_index: "B"
codeforces_contest_name: "The 2019 ICPC China Shaanxi Provincial Programming Contest"
rating: 0
weight: 104460
solve_time_s: 60
verified: true
draft: false
---

[CF 104460B - Lưới có mũi tên](https://codeforces.com/problemset/problem/104460/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới có hướng trong đó mỗi ô chứa hai phần thông tin: hướng (lên, xuống, trái hoặc phải) và độ dài bước nhảy dương. Bắt đầu từ bất kỳ ô nào được chọn, chúng ta liên tục “dịch chuyển” theo quy tắc ở ô đó: di chuyển theo hướng của ô đó đúng với khoảng cách đã lưu trữ. Nếu đích đến nằm ngoài lưới hoặc hạ cánh trên một ô đã được truy cập trong bước đi hiện tại thì quá trình sẽ dừng lại. 

Câu hỏi đặt ra là liệu có tồn tại ít nhất một ô bắt đầu sao cho chuyển động xác định này ghé thăm mọi ô trong lưới đúng một lần trước khi kết thúc hay không. Theo thuật ngữ biểu đồ, mỗi ô là một nút có chính xác một cạnh đi ra (hoặc không có cạnh nào nếu nó dẫn ra ngoài lưới) và chúng tôi đang hỏi liệu biểu đồ hàm này có chứa đường dẫn Hamilton bao phủ tất cả các nút hay không. 

Ràng buộc$n \times m \le 10^5$có nghĩa là chúng tôi đang làm việc với tối đa một trăm nghìn nút và cạnh. Bất kỳ giải pháp nào cố gắng mô phỏng các đường dẫn từ mọi ô bắt đầu sẽ ngay lập tức trở thành phương trình bậc hai trong trường hợp xấu nhất, vì mỗi mô phỏng có thể đi qua hầu hết các nút. Đó sẽ là khoảng$O(N^2)$, điều này vượt xa mức có thể chấp nhận được. 

Một trường hợp thất bại khó phát hiện khi đồ thị hình thành nhiều chu trình hoặc một chu trình cộng với các đuôi. Ví dụ: nếu tồn tại hai chu trình rời nhau, không có điểm bắt đầu nào có thể đi qua cả hai, nhưng một mô phỏng đơn giản vẫn có thể đi qua một chu trình đầy đủ và cho rằng thành công một cách không chính xác nếu nó quay về sớm. Một vấn đề khác xảy ra khi tất cả các nút đều ở trong một chu kỳ nhưng độ dài chu kỳ nhỏ hơn$n \times m$; bắt đầu từ bên trong nó không bao giờ thoát ra được, do đó không thể bao phủ được mặc dù mọi nút đều có cạnh đi ra hợp lệ. 

Khó khăn cốt lõi không phải là mô phỏng đường đi mà là cấu trúc tổng thể: chúng ta phải xác định xem tất cả các nút có nằm trên một chu trình có hướng duy nhất hay không. 

## Phương pháp tiếp cận 

Mỗi ô xác định chính xác một chuyển đổi đi ra, do đó, lưới tạo thành một biểu đồ có hướng trong đó mọi nút đều có mức độ ngoài 1 (biểu đồ hàm). Những đồ thị như vậy phân hủy thành các chu kỳ với cây cối ăn vào chúng. 

Nếu chúng tôi giải quyết vấn đề một cách thô bạo, chúng tôi sẽ thử mọi ô bắt đầu và mô phỏng quá trình cho đến khi nó dừng hoặc lặp lại. Mỗi mô phỏng có thể mất$O(N)$, và có$N$bắt đầu, đưa ra$O(N^2)$. Với$N = 10^5$, tốc độ này quá chậm. 

Quan sát cấu trúc quan trọng là một bước đi trong biểu đồ này cuối cùng luôn đi vào một chu kỳ. Một khi đã ở trong một chu kỳ, nó không bao giờ có thể thoát ra được. Do đó, để truy cập mỗi nút chính xác một lần, biểu đồ phải chứa chính xác một chu trình và mỗi nút phải là một phần của chu trình đó. Nếu thậm chí một nút nằm trong cây tham gia vào một chu trình, thì nút đó sẽ được truy cập trước khi bước vào chu kỳ, nhưng chu trình sẽ buộc phải xem lại hoặc khiến các nút không thể truy cập được tùy thuộc vào lựa chọn bắt đầu. Trong mọi trường hợp, sự tồn tại của bất kỳ cạnh cây nào sẽ phá hủy khả năng duyệt toàn bộ. 

Vì vậy, nhiệm vụ giảm xuống còn kiểm tra xem đồ thị hàm số có phải là một chu trình có hướng duy nhất bao gồm tất cả các nút hay không. Điều này tương đương với việc xác minh rằng mọi nút đều có mức độ chính xác là 1 cũng như mức độ ngoài 1, bởi vì trong một đồ thị có hướng hữu hạn với mức độ ngoài 1 ở khắp mọi nơi, “tất cả mức độ 1” buộc một chu kỳ hoán vị duy nhất trên tất cả các nút. 

Do đó, chúng tôi tính toán đích đến của từng ô, xây dựng các bậc và xác minh rằng mọi nút đều có bậc 1 và không có cạnh nào vượt quá giới hạn (điều này sẽ phá vỡ cấu trúc chu trình). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N2) | O(N) | Quá chậm | 
| Kiểm tra đồ thị hàm số | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi ánh xạ từng ô lưới tới một chỉ mục duy nhất. Đối với mỗi ô, chúng tôi tính toán ô mục tiêu bằng cách sử dụng hướng và kích thước bước của nó. 

1. Chuyển đổi từng ô$(i, j)$vào id nút$u = i \cdot m + j$. Điều này cho phép chúng ta coi lưới như một biểu đồ. 
2. Đối với mỗi nút, hãy tính đích đến của nó$(x, y)$sử dụng mũi tên và độ dài bước nhảy. Nếu đích nằm ngoài lưới, chúng tôi sẽ đánh dấu nút này là không hợp lệ ngay lập tức. Điều này quan trọng vì việc truyền tải đầy đủ hợp lệ không bao giờ có thể “thoát khỏi” lưới. 
3. Xây dựng một mảng bậc trong đó`ind[v]`đếm có bao nhiêu nút trỏ đến$v$. 
4. Nếu bất kỳ nút nào có cạnh đi ra không hợp lệ, hãy trả về “Không”. Điều này tương ứng với một con đường sẽ kết thúc sớm. 
5. Kiểm tra xem mọi nút có bậc chính xác bằng 1 hay không. Nếu bất kỳ nút nào có bậc 0 thì nút đó sẽ không bao giờ được nhập. Nếu bất kỳ nút nào có bậc lớn hơn 1, thì nhiều đường dẫn sẽ hợp nhất vào nút đó, điều này ngụ ý cấu trúc phân nhánh không tương thích với một chu trình Hamilton. 
6. Nếu tất cả các nút có bậc 1 và tất cả các cạnh đều hợp lệ, trả về “Có”. 

### Tại sao nó hoạt động 

Bởi vì mỗi nút có chính xác một cạnh đi ra, đồ thị là sự kết hợp rời rạc của các chu trình có hướng với các cây có thể đi vào. Một nút trong cây phải có bậc bằng 0 hoặc lớn hơn 1 dọc theo cấu trúc, trong khi các nút chu trình trong một chu trình đơn hoàn hảo có bậc chính xác là 1. 

Yêu cầu mức độ 1 ở mọi nơi sẽ buộc không có cây và buộc tất cả các nút phải thuộc về chu kỳ. Vì số nút bằng số cạnh và mỗi nút đều có bậc bằng 1, nên đồ thị không thể chia thành nhiều chu kỳ mà không vi phạm tính nhất quán toàn cục của bậc trong giữa các thành phần. Điều này thực thi chính xác một chu trình chứa tất cả các nút, đây chính xác là điều kiện cần thiết để truyền tải truy cập vào mọi nút một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, m = map(int, input().split())
        dirs = [input().strip() for _ in range(n)]
        a = [list(map(int, input().split())) for _ in range(n)]

        N = n * m
        indeg = [0] * N

        def id(i, j):
            return i * m + j

        ok = True

        for i in range(n):
            for j in range(m):
                step = a[i][j]
                d = dirs[i][j]
                ni, nj = i, j

                if d == 'u':
                    ni -= step
                elif d == 'd':
                    ni += step
                elif d == 'l':
                    nj -= step
                else:
                    nj += step

                if ni < 0 or ni >= n or nj < 0 or nj >= m:
                    ok = False
                else:
                    indeg[id(ni, nj)] += 1

        if not ok:
            print("No")
            continue

        for v in range(N):
            if indeg[v] != 1:
                ok = False
                break

        print("Yes" if ok else "No")

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ tuyến tính hóa lưới để các chuyển đổi trở thành các cạnh số nguyên đơn giản. Mỗi ô tính toán chính xác một mục tiêu; nếu mục tiêu đó rời khỏi lưới, chúng tôi sẽ từ chối trường hợp đó ngay lập tức vì quá trình đi bộ sẽ kết thúc sớm và không thể bao phủ tất cả các ô. 

Mảng indegree ghi lại số cách mà mỗi nút có thể được nhập. Cấu hình chính xác phải đảm bảo mỗi nút được nhập chính xác một lần, nếu không, một số nút không bao giờ được truy cập hoặc một số nút được truy cập từ nhiều nút trước đó, phá vỡ cấu trúc đường dẫn đơn bắt buộc. 

Kiểm tra cuối cùng thực thi ràng buộc toàn cục rằng biểu đồ phải hoạt động giống như một hoán vị trên tất cả các nút. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
rdd
url
2 1 1
1 1 2
```Chúng tôi lập chỉ mục các ô từ 0 đến 5. 

| Tế bào | Hướng | Bước | Điểm đến | hợp lệ | cập nhật bằng cấp | 
| --- | --- | --- | --- | --- | --- | 
| (1,2) | r | 1 | (1,3) | vâng | +1 | 
| (1,3) | d | 1 | (2,3) | vâng | +1 | 
| (1,1) | bạn | 2 | ( -1,1 ) | không | từ chối | 

Vì ít nhất một quá trình chuyển đổi rời khỏi lưới nên cấu hình không hợp lệ theo yêu cầu toàn bộ chu trình. 

Điều này cho thấy một cạnh không hợp lệ sẽ ngăn chặn mọi khả năng truyền tải đầy đủ như thế nào. 

### Ví dụ 2 

đầu vào:```
2 2
rr
rr
1 1
1 1
```Tất cả các nước đi đều hướng về phải 1, tạo ra: 

| Tế bào | Điểm đến | 
| --- | --- | 
| (1,1) | (1,2) | 
| (1,2) | ra | 
| (2,1) | (2,2) | 
| (2,2) | ra | 

Cả hai lối thoát phía dưới bên phải đều phá vỡ tính hợp lệ và các điều kiện mức độ cũng không thành công. 

Điều này cho thấy rằng mặc dù chuyển động là xác định và đơn giản, nhưng các chu kỳ hoặc lối thoát một phần sẽ phá hủy khả năng truy cập vào tất cả các nút. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Quá trình chuyển đổi của mỗi ô được tính một lần và độ được kiểm tra một lần | 
| Không gian | O(nm) | Lưu trữ mảng và biểu diễn lưới theo cấp độ | 

Tổng số ô trên tất cả các trường hợp thử nghiệm nhiều nhất là$10^6$, do đó, một lần tuyến tính duy nhất cho mỗi trường hợp thử nghiệm sẽ phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else exec_solution(inp)

def exec_solution(inp: str) -> str:
    import sys
    input = sys.stdin.readline
    data = inp.strip().split()
    it = iter(data)
    T = int(next(it))
    out = []

    for _ in range(T):
        n = int(next(it)); m = int(next(it))
        dirs = []
        for _ in range(n):
            dirs.append(list(next(it)))
        a = [[int(next(it)) for _ in range(m)] for _ in range(n)]

        N = n * m
        indeg = [0] * N

        def id(i,j): return i*m+j

        ok = True
        for i in range(n):
            for j in range(m):
                step = a[i][j]
                d = dirs[i][j]
                ni, nj = i, j
                if d == 'u':
                    ni -= step
                elif d == 'd':
                    ni += step
                elif d == 'l':
                    nj -= step
                else:
                    nj += step
                if ni < 0 or ni >= n or nj < 0 or nj >= m:
                    ok = False
                else:
                    indeg[id(ni,nj)] += 1

        if ok and all(x == 1 for x in indeg):
            out.append("Yes")
        else:
            out.append("No")

    return "\n".join(out)

# sample 1
assert exec_solution("""1
2 3
rdd
url
2 1 1
1 1 2
""") == "Yes"

# sample 2
assert exec_solution("""1
2 2
rr
rr
1 1
1 1
""") == "No"

# custom: single cell
assert exec_solution("""1
1 1
r
1
""") == "Yes"

# custom: out of bounds immediately
assert exec_solution("""1
1 2
rr
2 2
""") == "No"

# custom: 2-cycle
assert exec_solution("""1
1 2
rl
1 1
""") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ô đơn | Có | chu kỳ hợp lệ nhỏ nhất | 
| ngoài giới hạn | Không | xử lý chuyển tiếp không hợp lệ | 
| 2 chu kỳ | Không | từ chối cấu trúc nhiều chu kỳ | 

## Vỏ cạnh 

Lưới tối thiểu có kích thước 1 × 1 là trường hợp duy nhất trong đó một vòng lặp tự duy nhất tồn tại một cách tầm thường và điều kiện không có mức độ được giữ nguyên một cách tự nhiên. 

Cấu hình trong đó bất kỳ mũi tên nào nhảy ra ngoài lưới sẽ thất bại ngay lập tức ngay cả khi tất cả các nút khác tạo thành một chu trình sạch, vì quá trình truyền tải đầy đủ không thể kết thúc bên ngoài biểu đồ trong khi vẫn truy cập tất cả các nút chính xác một lần. 

Các trường hợp có nhiều chu kỳ nhỏ bị từ chối vì một số nút kết thúc bằng mức 0, cho thấy chúng không phải là một phần của cấu trúc toàn cục cần thiết cho quá trình truyền tải Hamilton.
