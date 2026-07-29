---
title: "CF 102756H - Nhà du hành"
description: "Bài toán mô phỏng một chuyến đi giữa các đảo. Mỗi hòn đảo có một vị trí trên mặt phẳng 2D và một số vật tư. Hòn đảo xuất phát là hòn đảo đầu tiên và đích đến là hòn đảo cuối cùng."
date: "2026-07-29T00:32:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102756
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 10-09-20 Div. 1"
rating: 0
weight: 102756
solve_time_s: 66
verified: true
draft: false
---

[CF 102756H - Du hành](https://codeforces.com/problemset/problem/102756/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô phỏng một chuyến đi giữa các đảo. Mỗi hòn đảo có một vị trí trên mặt phẳng 2D và một số vật tư. Hòn đảo xuất phát là hòn đảo đầu tiên và đích đến là hòn đảo cuối cùng. Thuyền của Kiana có sức chứa hành trình ban đầu bằng với lượng vật tư đã được sử dụng trên đảo xuất phát. Bất cứ khi nào đến một hòn đảo khác, cô ấy sẽ thu thập nguồn cung cấp ở đó, tăng khoảng cách tối đa mà cô ấy có thể đi trong một chuyến đi. 

Nhiệm vụ là quyết định xem có tồn tại một chuỗi các hòn đảo cho phép cô ấy đến đích hay không. Việc di chuyển không bị giới hạn ở một con đường đã được vẽ giữa các hòn đảo. Bạn có thể ghé thăm một hòn đảo bất cứ khi nào khoảng cách Euclide của nó với hòn đảo Kiana hiện đang ở tối đa sức chứa hiện tại của thuyền. 

Số lượng hòn đảo ít hơn 1000. Điều này ngay lập tức loại trừ việc khám phá mọi tuyến đường có thể, bởi vì số lượng đơn đặt hàng đảo có thể có có thể rất lớn. Giải pháp bậc hai có thể chấp nhận được vì 1000 bình phương chỉ bằng một triệu phép tính, rất nhỏ đối với giới hạn một giây. 

Các trường hợp chính xuất phát từ việc xử lý vấn đề giống như tìm kiếm đồ thị thông thường. Bản thân biểu đồ phụ thuộc vào số lượng vật tư được thu thập cho đến nay, do đó, việc xây dựng các cạnh ngay từ đầu có thể cho kết quả không chính xác. Ví dụ:```
3
0 0 5
4 0 10
10 0 0
```Đầu ra đúng là:```
YES
```Hòn đảo đầu tiên có thể đến hòn đảo thứ hai vì khoảng cách là 4. Sau khi thu thập thêm 10 vật tư, sức chứa sẽ trở thành 15 và có thể đến đích. Việc triển khai bất cẩn mà chỉ xem xét dung lượng ban đầu sẽ từ chối tuyến đường một cách không chính xác. 

Một trường hợp khác là một chuỗi trong đó các hòn đảo không có nguồn cung cấp vẫn cần thiết.```
3
0 0 3
3 0 0
6 0 0
```Đầu ra đúng là:```
YES
```Lần nhảy đầu tiên hoàn toàn có thể thực hiện được và lần nhảy thứ hai cũng có thể thực hiện được với cùng công suất. Việc triển khai chỉ xem xét các đảo tăng phạm vi sẽ bỏ lỡ các đường dẫn hợp lệ. 

Sai lầm phổ biến thứ ba là bỏ qua nguồn cung cấp của hòn đảo xuất phát hoặc thêm chúng hai lần.```
2
0 0 5
5 0 0
```Đầu ra đúng là:```
YES
```Đích đến chính xác là ở giới hạn ban đầu. Nguồn cung cấp ban đầu đã hoạt động và phải được tính một lần. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi con đường có thể từ hòn đảo xuất phát. Đối với mỗi hòn đảo hiện tại, nó sẽ thử tất cả các hòn đảo chưa sử dụng và tiếp tục đệ quy bất cứ khi nào có thể đạt được khoảng cách. Điều này đúng vì nó kiểm tra mọi chuỗi truy cập có thể xảy ra, nhưng số lượng tuyến đường tăng theo cấp số nhân. Với gần 1000 hòn đảo, thậm chí việc xem xét một phần rất nhỏ của tất cả các con đường có thể là không thể. 

Một cách hữu ích hơn để xem xét vấn đề là ngừng quan tâm đến thứ tự chính xác của các hòn đảo đã được thu thập. Tại bất kỳ thời điểm nào, thông tin duy nhất ảnh hưởng đến việc di chuyển trong tương lai là tập hợp các hòn đảo có thể tiếp cận hiện tại và tổng nguồn cung cấp được thu thập từ chúng. Vì nguồn cung cấp không bao giờ bị loại bỏ nên mỗi hòn đảo mới đến chỉ có thể giúp việc di chuyển trong tương lai trở nên dễ dàng hơn. 

Điều này tạo ra một quá trình mở rộng tham lam. Bắt đầu với hòn đảo đầu tiên được ghé thăm và liên tục thu thập mọi hòn đảo hiện có thể tiếp cận được. Mỗi hòn đảo được thu thập sẽ tăng sức chứa, điều này có thể tiết lộ nhiều hòn đảo có thể tiếp cận hơn. Quá trình kết thúc khi đích đến được thu thập hoặc không thể đến được hòn đảo mới. 

Lý do chúng ta không cần chọn một hòn đảo đặc biệt trước tiên là vì việc ghé thăm bất kỳ hòn đảo nào hiện có thể tiếp cận đều không gây hại gì. Nó chỉ bổ sung thêm nguồn cung và không bao giờ làm giảm số lượng các động thái có thể xảy ra trong tương lai. Việc trì hoãn một hòn đảo có thể tiếp cận không mang lại lợi ích gì vì sau này hòn đảo đó vẫn sẽ có sẵn với sức chứa tương tự hoặc lớn hơn. 

Việc thực hiện giữ khoảng cách tối thiểu từ mọi hòn đảo chưa được ghé thăm đến bất kỳ hòn đảo nào được ghé thăm. Bất cứ khi nào khoảng cách tối thiểu đó tối đa bằng công suất hiện tại, hòn đảo có thể được thêm vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N!) trong trường hợp xấu nhất | Ngăn xếp đệ quy O(N) | Quá chậm | 
| Tối ưu | O(N2) | O(N2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các đảo và tính toán trước bình phương khoảng cách giữa mỗi cặp đảo. Khoảng cách bình phương tránh được lỗi dấu phẩy động khi so sánh với sức chứa của thuyền. 
2. Đánh dấu hòn đảo bắt đầu là đã ghé thăm và đặt công suất hiện tại cho nguồn cung cấp của nó. Các nguồn cung cấp ban đầu đã được lắp đặt trên thuyền. 
3. Quét liên tục mọi hòn đảo chưa được ghé thăm. Nếu tồn tại một hòn đảo đã ghé thăm có khoảng cách nằm trong khả năng chứa hiện tại, hãy đánh dấu hòn đảo đó là đã ghé thăm và thêm nguồn cung cấp của nó vào dung lượng. 
4. Tiếp tục mở rộng cho đến khi không thể thêm hòn đảo mới hoặc điểm đến được ghé thăm. 
5. Đầu ra`YES`nếu đã đến đích, nếu không thì xuất ra`NO`. 

Lý do cho sự mở rộng tham lam này là hợp lý là vì các hòn đảo được ghé thăm tạo thành tập hợp lớn nhất có thể tiếp cận được với lượng vật tư thu thập được hiện tại. Nếu có thể đến được một hòn đảo tại một thời điểm nào đó, việc thu thập hòn đảo đó sớm hơn chỉ làm tăng sức chứa sẵn có. Thuật toán không bao giờ bỏ qua một cải tiến có thể có, vì vậy khi nó dừng lại, mọi hòn đảo chưa được ghé thăm thực sự không thể truy cập được. 

Tại sao nó hoạt động: 

Duy trì sự bất biến rằng mọi hòn đảo đã ghé thăm đều có thể truy cập được bằng cách sử dụng chính xác các đảo đã được đánh dấu là đã ghé thăm. Thuật toán chỉ thêm một hòn đảo sau khi xác minh rằng có thể đi thuyền tới đó, vì vậy bất biến vẫn đúng. Khi một hòn đảo được thêm vào, nguồn cung cấp của nó sẽ tăng sức chứa, nghĩa là mọi hành động có thể thực hiện được trước đây vẫn có thể thực hiện được. Nếu đích đến không bao giờ được thêm vào, quá trình quét cuối cùng sẽ chứng minh rằng không thể đến được hòn đảo chưa được ghé thăm nào từ khu vực có thể truy cập hiện tại, do đó không có sự tiếp tục nào tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n_line = input().strip()
    if not n_line:
        return
    n = int(n_line)

    islands = []
    for _ in range(n):
        x, y, s = map(int, input().split())
        islands.append((x, y, s))

    dist = [[0] * n for _ in range(n)]
    for i in range(n):
        xi, yi, _ = islands[i]
        for j in range(i + 1, n):
            xj, yj, _ = islands[j]
            d = (xi - xj) ** 2 + (yi - yj) ** 2
            dist[i][j] = d
            dist[j][i] = d

    visited = [False] * n
    visited[0] = True
    capacity = islands[0][2]
    remaining = n - 1

    while remaining:
        changed = False
        for i in range(1, n):
            if visited[i]:
                continue
            reachable = False
            for j in range(n):
                if visited[j] and dist[i][j] <= capacity * capacity:
                    reachable = True
                    break
            if reachable:
                visited[i] = True
                capacity += islands[i][2]
                remaining -= 1
                changed = True

        if visited[n - 1]:
            print("YES")
            return
        if not changed:
            break

    print("YES" if visited[n - 1] else "NO")

if __name__ == "__main__":
    solve()
```Phần đầu tiên tính bình phương khoảng cách. Việc sử dụng các giá trị bình phương sẽ tránh gọi căn bậc hai nhiều lần và tránh các vấn đề về độ chính xác khi khoảng cách nằm chính xác trên ranh giới. 

Mảng đã truy cập đại diện cho các hòn đảo có nguồn cung cấp đã được thu thập. Biến công suất là tổng số dặm tàu ​​hiện có thể đi được trong một chuyến đi liên tục. 

Vòng lặp chính thực hiện việc mở rộng tham lam. Mỗi đường chuyền cố gắng tìm những hòn đảo có thể đến được từ khu vực đã ghé thăm hiện tại. Khi một hòn đảo được thêm vào, nguồn cung cấp của hòn đảo đó sẽ ngay lập tức ảnh hưởng đến các lần kiểm tra sau này trong cùng một thẻ hoặc các thẻ trong tương lai. 

Việc so sánh sử dụng`dist[i][j] <= capacity * capacity`bởi vì`dist`lưu trữ khoảng cách bình phương. Trường hợp đẳng thức được đưa vào vì được phép đến một hòn đảo chính xác ở giới hạn của thuyền. 

## Ví dụ đã hoạt động 

Sử dụng mẫu:```
7
50 50 3
50 52 0
50 55 0
50 58 10
50 49 1
50 35 10
74 50 0
```| Bước | Đã ghé thăm đảo | Công suất | Hành động | 
| --- | --- | --- | --- | 
| 0 | {0} | 3 | Bắt đầu | 
| 1 | {0,1,4} | 4 | Đến các đảo trong khoảng cách 3 | 
| 2 | {0,1,2,4} | 4 | Tiếp cận đảo 2 thông qua việc mở rộng | 
| 3 | {0,1,2,3,4} | 14 | Thu thập vật tư từ đảo 3 | 
| 4 | {0,1,2,3,4,5} | 24 | Thu thập vật tư từ đảo 5 | 
| 5 | {0,1,2,3,4,5,6} | 24 | Đã đến đích | 

Dấu vết cho thấy tại sao các hòn đảo không có nguồn cung vẫn quan trọng. Quần đảo 1 và 2 tăng cường tập hợp các vị trí có thể tiếp cận mặc dù chúng không cải thiện năng lực. 

Một ví dụ về chuỗi nhỏ:```
3
0 0 3
3 0 0
6 0 0
```| Bước | Đã ghé thăm đảo | Công suất | Hành động | 
| --- | --- | --- | --- | 
| 0 | {0} | 3 | Bắt đầu | 
| 1 | {0,1} | 3 | Đảo 1 chính xác có thể truy cập được | 
| 2 | {0,1,2} | 3 | Điểm đến có thể truy cập chính xác | 

Điều này xác nhận rằng khoảng cách ranh giới được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N2) | Mỗi bản mở rộng sẽ kiểm tra các cặp đảo và có tối đa N lần bổ sung thành công. | 
| Không gian | O(N2) | Ma trận khoảng cách lưu trữ từng cặp đảo. | 

Với ít hơn 1000 hòn đảo, ma trận khoảng cách chứa khoảng một triệu mục, vừa vặn trong bộ nhớ. Việc quét bậc hai cũng đủ nhỏ cho các giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import io
import sys

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""7
50 50 3
50 52 0
50 55 0
50 58 10
50 49 1
50 35 10
74 50 0
""") == "YES\n", "sample 1"

assert run("""3
0 0 3
3 0 0
6 0 0
""") == "YES\n", "zero supply chain"

assert run("""2
0 0 1
5 0 0
""") == "NO\n", "unreachable destination"

assert run("""2
0 0 5
5 0 0
""") == "YES\n", "exact boundary distance"

assert run("""4
0 0 0
1 0 0
2 0 0
3 0 0
""") == "NO\n", "no initial movement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp mẫu | CÓ | Mở rộng đa đảo thông thường | 
| Chuỗi cung ứng bằng không | CÓ | Quần đảo không có nguồn cung cấp vẫn còn quan trọng | 
| Điểm đến quá xa | KHÔNG | Điều kiện dừng đúng | 
| Khớp khoảng cách chính xác | CÓ | So sánh ranh giới | 
| Không có chuyển động ban đầu | KHÔNG | Xử lý công suất khởi động | 

## Vỏ cạnh 

Đối với chuỗi cung ứng bằng không:```
3
0 0 3
3 0 0
6 0 0
```Thuật toán bắt đầu với dung lượng 3. Đảo 1 được thêm vào vì bình phương khoảng cách của nó là 9, bằng bình phương dung lượng. Sức chứa vẫn là 3, nhưng đảo 1 bây giờ là một vị trí mới, từ đó đảo 2 cũng ở khoảng cách 3. Đã đến đích. 

Đối với trường hợp nguồn cung cấp mới mở khóa một hòn đảo xa xôi:```
3
0 0 5
4 0 10
15 0 0
```Hòn đảo đầu tiên có thể đến hòn đảo thứ hai. Sau khi thu thập 10 nguồn cung cấp, sức chứa sẽ tăng lên 15, cho phép bạn đến được đích. Thuật toán phát hiện ra điều này vì hòn đảo mới ghé thăm sẽ ngay lập tức được đưa vào các lần kiểm tra khả năng tiếp cận sau này. 

Đối với chuyển động ranh giới chính xác:```
2
0 0 5
5 0 0
```Khoảng cách bình phương là 25 và dung lượng bình phương cũng là 25. Việc so sánh sử dụng`<=`, vì vậy hòn đảo được coi là có thể tiếp cận được. Một so sánh chặt chẽ sẽ tạo ra câu trả lời sai.
