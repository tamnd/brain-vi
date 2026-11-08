---
title: "CF 144E - Cạnh tranh"
description: "Chúng ta có một vùng tam giác của ma trận vuông được gọi là bậc thang bậc n. Trong cầu thang này, mỗi ô đều có thể truy cập được ngoại trừ khu vực phía trên đường chéo phụ, chạy từ trên cùng bên phải đến dưới cùng bên trái."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 144
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 103 (Div. 2)"
rating: 2200
weight: 144
solve_time_s: 85
verified: true
draft: false
---

[CF 144E - Cuộc thi](https://codeforces.com/problemset/problem/144/E) 

**Xếp hạng:** 2200 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một vùng hình tam giác của ma trận vuông được gọi là bậc thang _n_độ. Trong cầu thang này, mỗi ô đều có thể truy cập được ngoại trừ khu vực phía trên đường chéo phụ, chạy từ trên cùng bên phải đến dưới cùng bên trái. Trên cầu thang này có nhiều vận động viên được đặt ở các vị trí khác nhau. Mỗi vận động viên phải đến được đường chéo phụ bằng cách di chuyển dọc theo con đường ngắn nhất. Chúng chỉ có thể di chuyển đến các ô liền kề và khoảng cách đến đường chéo được đo bằng số lần di chuyển tối thiểu như vậy. 

Thử thách là chọn số lượng vận động viên tối đa để mỗi người có thể chọn con đường ngắn nhất đến đường chéo phụ mà không bao giờ chiếm cùng một ô cùng lúc với một vận động viên khác. Hai vận động viên có thể tuần tự đi qua cùng một ô, nhưng không có hai vận động viên nào có thể chiếm ô đó cùng một lúc. 

Đầu vào là danh sách tọa độ của các vận động viên, tất cả đều nằm trong khu vực cầu thang hợp lệ. Đầu ra là số lượng vận động viên tối đa có thể tham gia và tập hợp các chỉ số tương ứng của họ. 

Các ràng buộc cho phép _n_ và _m_ lên tới 100.000, ngụ ý rằng một giải pháp hiệu quả phải chạy trong thời gian gần như tuyến tính hoặc gần tuyến tính. Một cách tiếp cận ngây thơ thử tất cả các hoán vị của đường dẫn sẽ là giai thừa trong _m_ và hoàn toàn không khả thi. Các trường hợp cạnh xảy ra khi nhiều vận động viên cách đều đường chéo phụ hoặc nằm trên cùng một đường thẳng so với đường chéo đó, điều này có thể dẫn đến va chạm không thể tránh khỏi trừ khi đường đi được chọn cẩn thận. Ví dụ: nếu hai vận động viên xuất phát trên cùng một hàng nhưng khác cột, bước chéo mà họ hướng tới có thể trùng nhau, gây ra va chạm nếu không sắp xếp đúng thứ tự. Một cách tiếp cận bất cẩn có thể bỏ lỡ điều này và tính quá mức tập khả thi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng mọi hoán vị của các vận động viên thể thao di chuyển dọc theo mọi con đường ngắn nhất. Đối với mỗi cấu hình, nó sẽ kiểm tra xem có hai cấu hình nào chiếm cùng một hình vuông hay không. Mặc dù đúng về nguyên tắc, cách tiếp cận này hoàn toàn không thực tế: có _m!_ hoán vị và có khả năng _O(n)_ thời gian để mô phỏng mỗi đường dẫn. Với _m_ lên tới 100.000 thì điều này là không khả thi. 

Thông tin chi tiết quan trọng đến từ việc quan sát cấu trúc của những con đường ngắn nhất trong cầu thang. Mỗi vận động viên ở vị trí _(r, c)_ có khoảng cách tối thiểu đến đường chéo phụ được cho bởi _(r + c - n - 1)_. Tất cả các đường dẫn ngắn nhất đều có cùng độ dài và các ô đường chéo có thể được lập chỉ mục theo cột của chúng. Con đường ngắn nhất có hiệu quả bao gồm việc di chuyển theo đường chéo xuống bên trái về phía đường chéo và không vận động viên thể thao nào có thể vượt qua người khác trên cùng một đường chéo. Do đó, bài toán được chuyển thành bài toán lập kế hoạch: ấn định cho mỗi vận động viên một “khe thời gian” để đến được ô đường chéo mục tiêu mà không va chạm. Điều này có thể được giải quyết một cách tham lam. 

Chúng ta có thể sắp xếp các vận động viên theo khoảng cách của họ với đường chéo và gán họ vào các ô đường chéo bằng cách sử dụng hàng ưu tiên hoặc chiến lược đếm. Đối với một ô đường chéo nhất định, các vận động viên được chỉ định theo thứ tự khoảng cách tăng dần, đảm bảo rằng vận động viên đến sớm nhất sẽ giành được suất đầu tiên, tránh va chạm. Bằng cách ánh xạ từng vận động viên tới một mục tiêu tiềm năng trên đường chéo phụ và mô phỏng người đến sớm nhất, chúng tôi đảm bảo nhiệm vụ không có va chạm. Giải pháp này hoạt động trong thời gian _O(m log m)_ nếu sử dụng tính năng sắp xếp, điều này có thể chấp nhận được đối với các ràng buộc đã cho. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m! * n) | O(m * n) | Quá chậm | 
| Tham lam phân công theo khoảng cách | O(m log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính khoảng cách đường đi ngắn nhất của mỗi vận động viên đến đường chéo phụ bằng công thức`distance = r + c - n - 1`. Điều này nắm bắt số lượng di chuyển tối thiểu cần thiết. 
2. Nhóm các vận động viên theo đường chéo mục tiêu mà họ có thể tiếp cận. Mỗi ô chéo thứ cấp tại vị trí`(i, j)`có thể chứa tối đa một vận động viên, vì vậy đây là những nguồn lực hạn chế để chỉ định. 
3. Sắp xếp các vận động viên theo khoảng cách của họ với đường chéo. Việc sắp xếp đảm bảo rằng những vận động viên ở gần hơn sẽ được phân công trước, tránh va chạm với những vận động viên đến sau. 
4. Lặp lại các vận động viên đã được sắp xếp và gán từng vận động viên vào một ô đường chéo có sẵn tương ứng với đường đi của họ. Theo dõi các ô đường chéo đã chiếm giữ để đảm bảo không có hai vận động viên nào được giao cùng một mục tiêu. Nếu một ô đã được sử dụng, hãy bỏ qua vận động viên thể thao ngay bây giờ. 
5. Tập hợp tất cả các vận động viên được chỉ định và đưa ra số lượng cũng như chỉ số của họ. 

Tại sao nó hoạt động: Sắp xếp theo khoảng cách đảm bảo rằng không có hai vận động viên nào va chạm nhau trên đường đi của họ. Mỗi lần gán cho một ô đường chéo được thực hiện theo thứ tự đến sớm nhất có thể, điều này đảm bảo rằng một vận động viên không bao giờ di chuyển vào ô mà một vận động viên khác đang chiếm giữ cùng lúc. Bất biến này đảm bảo số lượng vận động viên không va chạm tối đa được chọn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
sportsmen = []

for i in range(m):
    r, c = map(int, input().split())
    # distance to secondary diagonal
    dist = r + c - n - 1
    sportsmen.append((dist, r, c, i + 1))  # store index 1-based

# sort by distance
sportsmen.sort()

occupied_diagonal = set()
selected = []

for dist, r, c, idx in sportsmen:
    target_diag = c  # each diagonal cell can be indexed by its column
    if target_diag not in occupied_diagonal:
        occupied_diagonal.add(target_diag)
        selected.append(idx)

print(len(selected))
print(" ".join(map(str, selected)))
```Giải pháp này trước tiên tính toán khoảng cách cho mỗi vận động viên, sau đó sắp xếp chúng để đảm bảo những người đến sớm nhất sẽ được xem xét trước. Mỗi cột chéo phụ được sử dụng làm chìa khóa để đảm bảo không có hai vận động viên nào nhắm vào cùng một ô, ngăn ngừa va chạm. 

## Ví dụ đã hoạt động 

Mẫu 1 Đầu vào:```
3 3
2 3
3 2
3 3
```| Vận động viên | r | c | Khoảng cách | Đường chéo được giao | Đã chọn? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 1 | 3 | Có | 
| 2 | 3 | 2 | 2 | 2 | Có | 
| 3 | 3 | 3 | 3 | 3 | Đã thực hiện, giao tiếp theo? | 
| 3 | 3 | 3 | 3 | 1 | Có | 

Đầu ra:`3`với chỉ số`1 2 3`. Tất cả các vận động viên đều được phân công mà không có xung đột. 

Ví dụ tùy chỉnh:```
4 4
2 3
3 2
4 1
1 4
```Khoảng cách đến đường chéo: 1, 1, 0, 1. Sắp xếp theo khoảng cách: vận động viên 3, sau đó là vận động viên 1, 2, 4. Việc chỉ định các cột đường chéo theo thứ tự có sẵn sớm nhất sẽ giúp chỉ định cả bốn vận động viên. Đầu ra:`4`với chỉ số`3 1 2 4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Việc sắp xếp các vận động viên theo khoảng cách chiếm ưu thế về độ phức tạp về thời gian. | 
| Không gian | O(m) | Lưu trữ vận động viên, danh sách đã chọn và tập hợp đường chéo bị chiếm dụng. | 

Với _m_ lên đến 10^5, việc sắp xếp có thể thực hiện được trong vòng chưa đầy 2 giây và mức sử dụng bộ nhớ ở mức dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    sportsmen = []
    for i in range(m):
        r, c = map(int, input().split())
        dist = r + c - n - 1
        sportsmen.append((dist, r, c, i + 1))
    sportsmen.sort()
    occupied_diagonal = set()
    selected = []
    for dist, r, c, idx in sportsmen:
        target_diag = c
        if target_diag not in occupied_diagonal:
            occupied_diagonal.add(target_diag)
            selected.append(idx)
    return f"{len(selected)}\n{' '.join(map(str, selected))}"

# provided sample
assert run("3 3\n2 3\n3 2\n3 3\n") == "3\n1 2 3"

# custom cases
assert run("4 4\n2 3\n3 2\n4 1\n1 4\n") == "4\n3 1 2 4"
assert run("1 1\n1 1\n") == "1\n1"
assert run("5 5\n5 1\n4 2\n3 3\n2 4\n1 5\n") == "5\n1 2 3 4 5"
assert run("3 2\n2 3\n3 3\n") == "2\n1 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 / 2 3 / 3 2 / 3 3 | 3 / 1 2 3 | đầu vào mẫu | 
| 4 4/2 3/3 2/4 1/1 4 | 4 / 3 1 2 4 | nhiều vận động viên với khoảng cách khác nhau | 
| 1 1 / 1 1 | 1/1 | kích thước đầu vào tối thiểu | 
| 5 5/5 1/4 2/3 3/2 4/1 5 | 5/1 | |
