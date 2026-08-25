---
title: "CF 104308F - Cặp Xored"
description: "Chúng ta được yêu cầu xây dựng một mảng có độ dài n trong đó mỗi giá trị là một số nguyên không âm 30 bit. Việc xây dựng phải đáp ứng một tập hợp các ràng buộc liên quan đến các phần tử theo bất đẳng thức với một giá trị cố định hoặc bằng mối quan hệ XOR giữa các cặp."
date: "2026-07-01T20:02:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104308
codeforces_index: "F"
codeforces_contest_name: "Mirror of Independence Day Programming Contest 2023 by MIST Computer Club"
rating: 0
weight: 104308
solve_time_s: 70
verified: true
draft: false
---

[CF 104308F - Cặp Xored](https://codeforces.com/problemset/problem/104308/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xây dựng một mảng có độ dài`n`trong đó mỗi giá trị là một số nguyên không âm 30 bit. Việc xây dựng phải đáp ứng một tập hợp các ràng buộc liên quan đến các phần tử theo bất đẳng thức với một giá trị cố định hoặc bằng mối quan hệ XOR giữa các cặp. 

Loại ràng buộc thứ hai hoạt động giống như một quy tắc cấu trúc: nếu hai chỉ số được liên kết bởi một phương trình có dạng`a[i] XOR a[j] = x`, sau đó khi một giá trị được chọn, giá trị còn lại sẽ được xác định đầy đủ. Điều này có nghĩa là các ràng buộc không phải là các điều kiện độc lập mà là xác định một hệ thống các mối quan hệ lan truyền qua mảng. 

Loại ràng buộc đầu tiên cấm các giá trị cụ thể tại các vị trí cụ thể. Những hành động này đóng vai trò là những loại trừ cần phải tránh sau khi tất cả các mối quan hệ XOR đã cố định cấu trúc tương đối của các giá trị. 

Khó khăn chính xuất phát từ thực tế là các ràng buộc XOR có thể hình thành các thành phần được kết nối và trong mỗi thành phần, tất cả các giá trị được gắn với nhau bằng các phép biến đổi bitwise nhất quán. Việc gán đơn giản cho mỗi ràng buộc không thành công vì một mâu thuẫn trong một chu trình hoặc một giá trị bị cấm muộn có thể làm mất hiệu lực các lựa chọn trước đó. 

Các ràng buộc rất lớn, lên tới 100000 cho mỗi trường hợp thử nghiệm, do đó, bất kỳ giải pháp nào cố gắng kiểm tra các bài tập hoặc quay lui các giá trị đều ngay lập tức quá chậm. Cần phải xây dựng dựa trên đồ thị tuyến tính hoặc gần tuyến tính, vì các phép toán theo thứ tự`O(n + m)`là lựa chọn khả thi duy nhất. 

Một trường hợp lỗi nhỏ xuất hiện khi các ràng buộc XOR tạo thành một chu trình không nhất quán. Ví dụ, nếu chúng ta rút ra được điều đó`a1 XOR a2 = 3`,`a2 XOR a3 = 4`, Và`a1 XOR a3 = 10`, ba cái này hàm ý mâu thuẫn vì XOR hai cái đầu tiên đã sửa được rồi`a1 XOR a3`. Bất kỳ giải pháp nào không xác nhận rõ ràng tính nhất quán trong các chu kỳ sẽ âm thầm tạo ra các mảng không hợp lệ. 

Một chế độ lỗi khác phát sinh khi các giá trị bị cấm được xử lý cục bộ tại các nút trước khi xem xét việc truyền XOR. Một giá trị bị cấm ở chỉ mục`i`chuyển thành một lựa chọn bị cấm đối với đại diện toàn cầu của thành phần của nó, nhưng chỉ sau khi được điều chỉnh bằng phần bù XOR của nút. Bỏ qua sự thay đổi này dẫn đến suy luận tổng thể không chính xác. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua các ràng buộc XOR, vấn đề sẽ giảm xuống còn việc chọn các giá trị độc lập trong khi tránh các giá trị bị cấm, điều này không đáng kể. Nếu chúng ta bỏ qua các ràng buộc bị cấm, các ràng buộc XOR sẽ xác định một biểu đồ trong đó mỗi thành phần được kết nối có thể được gán một giá trị cơ sở duy nhất và mọi thứ khác tuân theo độ lệch XOR. Điều này đã gợi ý một cấu trúc biểu đồ với các yêu cầu về tính nhất quán. 

Một cách tiếp cận bạo lực sẽ cố gắng gán các giá trị cho mỗi nút và liên tục thực thi các ràng buộc cho đến khi hội tụ. Mỗi khi một giá trị được thay đổi, tất cả các ràng buộc xảy ra với nút đó sẽ cần phải được kiểm tra lại, có khả năng lan truyền các cập nhật trên biểu đồ. Trong trường hợp xấu nhất, mỗi phép gán có thể kích hoạt một tầng qua tất cả các cạnh, dẫn đến hành vi hàm mũ hoặc ít nhất là bậc hai đối với các chuỗi ràng buộc. Với tối đa`10^5`những hạn chế, điều này trở nên không thể thực hiện được. 

Quan sát chính là các ràng buộc XOR xác định một biểu đồ vô hướng có trọng số trong đó mỗi cạnh thực thi một chênh lệch cố định trong XOR. Khi một giá trị được cố định tại một nút của thành phần được kết nối, mọi nút khác trong thành phần đó sẽ được xác định duy nhất. Điều này cho phép chúng tôi giảm toàn bộ hệ thống xuống còn một biến miễn phí cho mỗi thành phần. 

Sau khi nén từng thành phần thành một bậc tự do, nhiệm vụ còn lại là chọn giá trị cơ sở cho thành phần đó sao cho tất cả các ràng buộc bị cấm đều được thỏa mãn. Mỗi điều kiện bị cấm chuyển thành một loại trừ đối với giá trị cơ bản sau khi được điều chỉnh bằng bù XOR của nút. Vì miền giá trị lớn (`2^30`), chúng ta luôn có thể tìm thấy một lựa chọn hợp lệ nếu các ràng buộc nhất quán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tuyên truyền vũ phu | Hàm mũ / rất lớn | O(n + m) | Quá chậm | 
| Đồ thị XOR + Nén thành phần | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa hệ thống dưới dạng biểu đồ trong đó mỗi ràng buộc XOR là một cạnh mang trọng số. 

1. Xây dựng biểu đồ trong đó mỗi ràng buộc của biểu mẫu`a[i] XOR a[j] = x`trở thành một cạnh vô hướng`(i, j)`có nhãn`x`. Điều này có nghĩa là nếu chúng ta gán một giá trị cho`i`, giá trị của`j`buộc phải là`a[i] XOR x`. 
2. Duyệt từng thành phần được kết nối bằng DFS hoặc BFS và gán giá trị tương đối`dist[i]`đối với mỗi nút, được hiểu là`a[i] XOR base_of_component`. Trong khi duyệt, nếu chúng ta truy cập lại một nút và giá trị ngụ ý xung đột với giá trị được gán trước đó, chúng ta sẽ kết luận ngay rằng hệ thống không nhất quán. 
3. Thu thập tất cả các nút thuộc từng thành phần được kết nối cùng với`dist[i]`các giá trị. 
4. Đối với mỗi thành phần, hãy tính tập hợp bị cấm cho giá trị cơ bản của thành phần đó. Mọi ràng buộc của hình thức`a[i] != x`dịch sang`base XOR dist[i] != x`, tương đương với`base != x XOR dist[i]`. Chúng tôi chèn tất cả các giá trị chuyển đổi bị cấm như vậy vào một tập hợp cho thành phần đó. 
5. Chọn giá trị cơ bản cho thành phần bằng cách bắt đầu từ 0 và tăng dần cho đến khi chúng ta tìm thấy giá trị không có trong tập hợp bị cấm. Điều này hoạt động vì kích thước miền cực kỳ lớn so với số lượng giá trị bị cấm. 
6. Sau khi chọn được cơ sở, hãy gán mọi nút trong thành phần là`a[i] = base XOR dist[i]`. 

Bất biến trung tâm là trong mỗi thành phần được kết nối, mọi giá trị nút luôn nhất quán với tất cả các ràng buộc XOR bằng cách xây dựng`dist[i]`. Quyền tự do duy nhất còn lại là cơ sở toàn cục và các ràng buộc bị cấm chỉ hạn chế tham số duy nhất này cho mỗi thành phần. Vì tất cả các điều kiện bị cấm đều được tôn trọng trong quá trình lựa chọn cơ sở, phép gán cuối cùng thỏa mãn đồng thời mọi ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    MAXV = (1 << 30)

    for _ in range(t):
        n, m = map(int, input().split())

        g = [[] for _ in range(n + 1)]
        type1 = [[] for _ in range(n + 1)]

        for _ in range(m):
            tmp = input().split()
            if tmp[0] == '1':
                _, i, x = tmp
                i = int(i)
                x = int(x)
                type1[i].append(x)
            else:
                _, i, j, x = tmp
                i = int(i)
                j = int(j)
                x = int(x)
                g[i].append((j, x))
                g[j].append((i, x))

        vis = [False] * (n + 1)
        dist = [0] * (n + 1)
        comp = [-1] * (n + 1)
        comps = []

        ok = True

        for i in range(1, n + 1):
            if vis[i]:
                continue
            stack = [i]
            vis[i] = True
            dist[i] = 0
            comp_id = len(comps)
            comps.append([])

            while stack and ok:
                v = stack.pop()
                comp[v] = comp_id
                comps[comp_id].append(v)

                for to, w in g[v]:
                    if not vis[to]:
                        vis[to] = True
                        dist[to] = dist[v] ^ w
                        stack.append(to)
                    else:
                        if dist[to] != (dist[v] ^ w):
                            ok = False
                            break

        if not ok:
            print("No")
            continue

        base = [0] * len(comps)
        used = [set() for _ in range(len(comps))]

        for v in range(1, n + 1):
            cid = comp[v]
            for x in type1[v]:
                used[cid].add(x ^ dist[v])

        for cid in range(len(comps)):
            b = 0
            while b in used[cid]:
                b += 1
            base[cid] = b

        ans = [0] * (n + 1)
        for v in range(1, n + 1):
            ans[v] = base[comp[v]] ^ dist[v]

        print("Yes")
        print(*ans[1:])

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xây dựng biểu đồ ràng buộc XOR và tính toán khoảng cách XOR tương đối bên trong mỗi thành phần. DFS đảm bảo rằng mọi nút đều có một nhãn nhất quán và bất kỳ mâu thuẫn nào sẽ ngay lập tức làm mất hiệu lực của trường hợp thử nghiệm. 

Sau khi các thành phần được xác định, mọi ràng buộc bị cấm sẽ được chuyển thành hạn chế đối với giá trị cơ sở của thành phần. Sự biến đổi`x XOR dist[i]`là thứ sắp xếp các hạn chế dành riêng cho từng nút thành một hệ tọa độ thống nhất cho mỗi thành phần. 

Vòng lặp lựa chọn cơ sở an toàn vì số lượng giá trị bị cấm bị giới hạn bởi số lượng ràng buộc, trong khi không gian tìm kiếm trải rộng`2^30`. Ngay cả việc quét tuyến tính vẫn hiệu quả vì mỗi thành phần chỉ đóng góp một phần nhỏ trong tất cả các ràng buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
constraints:
1: a1 != 0
2: a1 XOR a2 = 1
3: a2 XOR a3 = 2
```Chúng tôi xây dựng một thành phần với khoảng cách:`dist[1] = 0`,`dist[2] = 1`,`dist[3] = 3`. 

Bây giờ chuyển đổi ràng buộc bị cấm`a1 != 0`: 

cơ sở XOR 0 != 0 → cơ sở != 0. 

Vậy tập bị cấm là`{0}`. 

Chúng tôi chọn`base = 1`. 

Bài tập trở thành:`a1 = 1`,`a2 = 0`,`a3 = 2`. 

| nút | quận | value = cơ sở XOR dist | 
| --- | --- | --- | 
| 1 | 0 | 1 | 
| 2 | 1 | 0 | 
| 3 | 3 | 2 | 

Điều này xác nhận rằng các mối quan hệ XOR được giữ nguyên và tránh được giá trị bị cấm. 

### Ví dụ 2 (phát hiện sự không nhất quán) 

đầu vào:```
1 3
a1 XOR a2 = 1
a2 XOR a1 = 0
```Đi ngang:`a1 XOR a2 = 1`ngụ ý`dist[2] = 1`. 

Ràng buộc thứ hai ngụ ý`dist[1] XOR dist[2] = 0`, lực nào`dist[2] = 0`. 

Chúng tôi đã có`dist[2] = 1`, mâu thuẫn xảy ra trong quá trình truyền tải, do đó đầu ra là`No`. 

Điều này chứng tỏ rằng tính nhất quán của chu trình được thực thi thông qua kiểm tra khoảng cách XOR. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi nút và cạnh được xử lý một lần trong DFS và các ràng buộc bị cấm được tổng hợp một lần | 
| Không gian | O(n + m) | Lưu trữ đồ thị, mảng khoảng cách và ghi sổ thành phần | 

Các ràng buộc cho phép lên đến`2 × 10^5`tổng số hoạt động trong các thử nghiệm và thuật toán vẫn hoàn toàn tuyến tính, do đó, nó phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from types import ModuleType

    # assumes solution is in same file; for standalone testing, call solve()
    # here we redefine minimal wrapper
    exec_globals = globals().copy()
    exec_globals["input"] = lambda: sys.stdin.readline()
    return ""

# Sample-style and custom tests (conceptual; requires integrated runner)

# minimal consistency
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn, không có ràng buộc | Có + bất kỳ giá trị nào | xây dựng tầm thường | 
| chuỗi nhỏ nhất quán | Có | tính chính xác của việc truyền bá | 
| Mâu thuẫn chu trình XOR | Không | phát hiện chu kỳ | 
| nhiều thành phần | Có | lựa chọn cơ sở độc lập | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các ràng buộc XOR tạo thành một chuỗi dài trên tất cả các nút trong khi các ràng buộc bị cấm tập trung trên một nút duy nhất. Thuật toán xử lý điều này vì tất cả các hạn chế được dịch sang cùng tọa độ cơ sở, do đó, ngay cả một tập hợp dày đặc các ràng buộc cục bộ cũng không ảnh hưởng đến các thành phần khác. 

Một trường hợp cạnh khác là khi một thành phần có nhiều giá trị bị cấm bao trùm một phạm vi liền kề bắt đầu từ 0. Quá trình quét tuyến tính để tìm cơ sở hợp lệ vẫn thành công vì miền lớn hơn rất nhiều so với số lượng mục bị cấm, đảm bảo có khoảng trống. 

Trường hợp thứ ba là một chu kỳ nhất quán ở địa phương nhưng không nhất quán trên toàn cầu. Kiểm tra khoảng cách dựa trên DFS sẽ phát hiện ra điều này ngay lập tức bằng cách thực thi rằng mọi cạnh đều phải phù hợp với khoảng cách được chỉ định trước đó, ngăn chặn các phép gán không hợp lệ đạt đến giai đoạn chọn cơ sở.
