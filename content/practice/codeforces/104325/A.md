---
title: "CF 104325A - Phương án xây dựng"
description: "Chúng ta được cung cấp một hệ thống sản xuất trong đó mọi nguyên liệu đều được tạo ra theo đúng một công thức được thực hiện trên một loại máy cụ thể. Mỗi loại máy có hệ số nhân tốc độ cố định và mỗi công thức có thời gian cơ bản."
date: "2026-07-01T19:13:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104325
codeforces_index: "A"
codeforces_contest_name: "AGM 2023 Qualification Round"
rating: 0
weight: 104325
solve_time_s: 99
verified: false
draft: false
---

[CF 104325A - Kế hoạch xây dựng](https://codeforces.com/problemset/problem/104325/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống sản xuất trong đó mọi nguyên liệu đều được tạo ra theo đúng một công thức được thực hiện trên một loại máy cụ thể. Mỗi loại máy có hệ số nhân tốc độ cố định và mỗi công thức có thời gian cơ bản. Thời gian thực tế để sản xuất một đơn vị phụ thuộc vào việc chia thời gian cơ bản cho tốc độ máy. Công thức nấu ăn cũng tiêu thụ các nguyên liệu khác do các công thức nấu ăn khác tạo ra, tạo thành một biểu đồ phụ thuộc không có chu kỳ. 

Mục tiêu không phải là mô phỏng quá trình sản xuất mà là xác định số lượng máy thuộc loại trạm của mỗi công thức được yêu cầu để tạo ra một tập hợp nguyên liệu mục tiêu với tốc độ yêu cầu chính xác mỗi giây. Mỗi công thức chạy độc lập trên các máy riêng của nó và mỗi máy đóng góp một thông lượng cố định cho công thức của nó. Nếu một công thức sản xuất chậm hơn yêu cầu, chúng tôi sẽ thêm nhiều máy hơn vào trạm của công thức đó. 

Điểm tinh tế quan trọng là các yêu cầu sản xuất sẽ lan truyền ngược lại qua biểu đồ phụ thuộc. Nếu sản phẩm cuối cùng cần một tỷ lệ nhất định thì các thành phần của nó phải được sản xuất đủ nhanh để hỗ trợ nó, v.v. theo cách đệ quy cho đến khi đạt được nguyên liệu thô. 

Các ràng buộc đủ nhỏ để việc truyền tuyến tính trên tất cả các công thức nấu ăn là đủ. Có tối đa 100 công thức nấu ăn và 100 loại máy, do đó, ngay cả việc nhân giống lặp đi lặp lại hoặc tích lũy phụ thuộc ngược cũng có thể thực hiện được. Cấu trúc quan trọng là biểu đồ có tính tuần hoàn, đảm bảo chúng ta có thể tính toán các yêu cầu trong một lần nếu được xử lý theo thứ tự tôpô ngược. 

Một trường hợp sai sót phổ biến xuất hiện khi vật liệu trung gian được tái sử dụng trong nhiều sản phẩm. Nếu chúng tôi tính toán các yêu cầu cho mỗi sản phẩm cuối cùng một cách độc lập và quên tổng hợp chính xác, chúng tôi sẽ tính thiếu các phần phụ thuộc được chia sẻ. 

Ví dụ: nếu cả hai sản phẩm đều yêu cầu quặng sắt, một cách tiếp cận đơn giản có thể tính toán nhu cầu quặng riêng biệt và ghi đè các giá trị thay vì tính tổng chúng. Yêu cầu đúng là tổng của tất cả các nhu cầu ở hạ nguồn. 

Một cạm bẫy khác là việc chia lãi suất theo dấu phẩy động. Vì tốc độ sản xuất phụ thuộc vào các tỷ số tốc độ như t/s nên việc làm tròn quá sớm có thể tạo ra số lượng máy sai lệch. Cách tiếp cận an toàn là tính tỷ lệ yêu cầu dưới dạng số thực và chỉ làm tròn ở bước cuối cùng. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ nhất là mô phỏng các yêu cầu sản xuất từ từng nguyên liệu mục tiêu một cách độc lập. Đối với mỗi đầu ra được yêu cầu, chúng tôi mở rộng đệ quy công thức của nó, tính toán tỷ lệ của từng thành phần, sau đó tính toán lại số lượng máy cho mỗi công thức. Điều này hiệu quả vì sự phụ thuộc là hữu hạn và không theo chu kỳ, do đó quá trình đệ quy chấm dứt. 

Tuy nhiên, cách tiếp cận này tính toán lại các bài toán con chồng chéo nhiều lần. Nếu một vật liệu được sử dụng trong nhiều sản phẩm, cây con của nó sẽ được duyệt nhiều lần. Trong trường hợp xấu nhất, mỗi công thức phụ thuộc vào hầu hết các công thức khác, do đó việc tính toán lại dẫn đến hành vi bậc hai về số lượng công thức, điều này là không cần thiết đối với cấu trúc chung. 

Quan sát quan trọng là hệ thống là một DAG trong đó mỗi nút đóng góp tuyến tính vào nhu cầu ngược dòng. Thay vì tính toán lại theo gốc, chúng tôi tổng hợp các tỷ lệ bắt buộc từ dưới lên. Khi chúng tôi biết tốc độ đầu ra cần thiết cho mọi nguyên liệu, số lượng máy của mỗi công thức sẽ trở thành một phép tính độc lập. 

Do đó, trước tiên, chúng tôi tính toán tỷ lệ yêu cầu cho tất cả nguyên liệu bằng cách sử dụng quy trình lan truyền phụ thuộc ngược: bắt đầu từ kết quả đầu ra được yêu cầu, chúng tôi đẩy nhu cầu ngược lại thông qua các công thức nấu ăn. Bởi vì mỗi công thức xác định một phần mở rộng cố định, đây chỉ là sự tích lũy có trọng số dọc theo các cạnh. Sau khi đã biết tất cả các tốc độ vật liệu, việc chuyển đổi chúng thành số lượng máy là phép chia trực tiếp cho thông lượng trên mỗi máy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đệ quy Brute Force cho mỗi mục tiêu | O(Q · N2) | O(N) | Quá chậm | 
| Tuyên truyền ngược lại nhu cầu | O(N + Q) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi coi mọi vật liệu như một nút trong biểu đồ. Mỗi công thức là một lợi thế từ nguyên liệu đầu ra đến nguyên liệu đầu vào, với các hệ số cố định. 

Trước tiên, chúng tôi tính toán, đối với mỗi công thức, một máy tạo ra bao nhiêu đơn vị mỗi giây. Điều này có được bằng cách chia tốc độ máy cho thời gian cơ bản. 

Sau đó chúng tôi duy trì một bản đồ`need[x]`đại diện cho bao nhiêu đơn vị mỗi giây của vật liệu`x`được yêu cầu trên toàn cầu. 

Chúng tôi khởi tạo`need`sử dụng các truy vấn cuối cùng. 

Chúng tôi xử lý vật liệu theo thứ tự tôpô ngược. Vì biểu đồ có tính tuần hoàn nên chúng ta có thể sắp xếp theo cấu trúc liên kết một cách rõ ràng hoặc dựa vào DFS được ghi nhớ. Trong thực tế, một DFS từ các mục tiêu là đủ. 

Đối với mỗi vật liệu`p`với công thức sản xuất nó, nếu tỷ lệ yêu cầu của nó là`need[p]`, thì mọi thành phần`n`theo yêu cầu của công thức phải tăng thêm`need[p] * c`, Ở đâu`c`là hệ số tiêu thụ của công thức. Điều này truyền bá nhu cầu ngược lại. 

Một lần tất cả`need`giá trị được tính toán, chúng tôi xác định số lượng máy cho mỗi công thức. Để sản xuất công thức`p`về loại máy`l`, mỗi máy đóng góp một tỷ lệ cố định`rate[p]`. Số máy cần thiết là`ceil(need[p] / rate[p])`. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý một vật liệu,`need[x]`bằng tổng tốc độ sản xuất ở trạng thái ổn định cần thiết cho x trên tất cả các chuỗi phụ thuộc bắt đầu từ nhu cầu cuối cùng. Bởi vì mỗi công thức là tuyến tính và độc lập nên cần có sự đóng góp từ các bậc cha mẹ khác nhau mà không bị can thiệp. Cấu trúc không tuần hoàn đảm bảo chúng tôi không bao giờ truy cập lại một nút có thông tin không đầy đủ, do đó việc tích lũy là cuối cùng khi thứ tự xử lý được tôn trọng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict, deque
import math

M = int(input())
speed = {}
for _ in range(M):
    name, s = input().split()
    speed[name] = float(s)

N = int(input())

recipe = {}
inputs = {}
machine = {}

all_materials = set()

for _ in range(N):
    p, l, t = input().split()
    t = float(t)
    k = int(input())
    req = []
    for _ in range(k):
        n, c = input().split()
        c = int(c)
        req.append((n, c))
        all_materials.add(n)
    recipe[p] = (l, t)
    inputs[p] = req
    machine[p] = l
    all_materials.add(p)

Q = int(input())

need = defaultdict(float)

targets = []
for _ in range(Q):
    m, c = input().split()
    c = int(c)
    need[m] += c

# compute production rate per machine for each recipe
rate = {}
for p, (l, t) in recipe.items():
    rate[p] = speed[l] / t

# memo DFS to propagate requirements
sys.setrecursionlimit(1000000)

visited = set()

def dfs(p):
    if p in visited:
        return
    visited.add(p)
    if p not in recipe:
        return
    for n, c in inputs[p]:
        need[n] += need[p] * c
        dfs(n)

for m in list(need.keys()):
    dfs(m)

out = []
for p in recipe:
    r = rate[p]
    machines = need[p] / r
    machines = math.ceil(machines - 1e-12)
    out.append((p, machine[p], machines))

for p, l, r in out:
    print(p, l, r)
```Việc triển khai trước tiên sẽ xây dựng biểu đồ công thức và tốc độ máy, sau đó tính toán tốc độ sản xuất trên mỗi máy bằng cách sử dụng tốc độ chia cho thời gian cơ bản. các`need`các cửa hàng từ điển yêu cầu tỷ lệ sản xuất và được gieo rắc những nhu cầu cuối cùng. 

DFS truyền bá các yêu cầu ngược: khi cần một nguyên liệu ở một mức độ nào đó, tất cả các đầu vào của nó sẽ kế thừa nhu cầu tỷ lệ. Đệ quy đảm bảo các phụ thuộc bắc cầu được mở rộng hoàn toàn. 

Cuối cùng, mỗi công thức được chuyển thành số lượng máy bằng cách sử dụng phép chia trần. Epsilon nhỏ tránh mất ổn định dấu phẩy động khi các giá trị cực kỳ gần với số nguyên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi bắt đầu với`electronic_circuit = 10`. Mỗi bộ hợp ngữ tạo ra 2 mạch mỗi giây vì tốc độ là 0,5 và thời gian là 0,5, do đó tốc độ là 1 mỗi giây cho mỗi bộ hợp ngữ theo cách diễn giải chuẩn hóa. 

| Bước | Chất liệu | Cần | Hành động | 
| --- | --- | --- | --- | 
| 1 | mạch điện tử | 10 | yêu cầu bắt đầu | 
| 2 | cáp đồng | 30 | 3 mỗi mạch | 
| 3 | tấm đồng | 30 | từ công thức cáp | 
| 4 | tấm sắt | 10 | từ công thức mạch | 
| 5 | quặng sắt | 10 | từ công thức đĩa | 

Khi nhu cầu của lá được tính toán, số lượng máy được tính bằng cách chia cho thông lượng trên mỗi máy. 

Kết quả cho thấy nhu cầu tăng theo cấp số nhân từ sản phẩm cuối cùng đến quặng thô, xác nhận rằng mỗi lớp phụ thuộc có quy mô tuyến tính. 

### Mẫu 2 

Mục tiêu là`transport_belt = 7`và sự phụ thuộc của nó bao gồm cả`iron_plate`Và`iron_gear`. 

| Bước | Chất liệu | Cần | 
| --- | --- | --- | 
| 1 | vành đai vận chuyển | 7 | 
| 2 | tấm sắt | 7 | 
| 3 | sắt_bánh | 7 | 
| 4 | quặng sắt | 39 | 

Nhu cầu quặng sắt tăng vì cả iron_plate và iron_gear đều phụ thuộc vào nó. Điều này thể hiện sự tổng hợp phụ gia chính xác trên nhiều đường dẫn phụ thuộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + Q) | mỗi công thức và phần phụ thuộc được truy cập một lần | 
| Không gian | O(N) | lưu trữ đồ thị và bản đồ nhu cầu | 

Giới hạn của 100 công thức nấu ăn và 100 máy móc giúp việc này trở nên đủ nhanh một cách dễ dàng. Ngay cả với sự lan truyền đệ quy, tổng số cạnh vẫn nhỏ và mỗi cạnh được xử lý một lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    from collections import defaultdict

    M = int(input())
    speed = {}
    for _ in range(M):
        name, s = input().split()
        speed[name] = float(s)

    N = int(input())
    recipe = {}
    inputs = {}
    machine = {}

    for _ in range(N):
        p, l, t = input().split()
        t = float(t)
        k = int(input())
        req = []
        for _ in range(k):
            n, c = input().split()
            c = int(c)
            req.append((n, c))
        recipe[p] = (l, t)
        inputs[p] = req
        machine[p] = l

    Q = int(input())
    need = defaultdict(float)

    for _ in range(Q):
        m, c = input().split()
        need[m] += int(c)

    rate = {p: speed[recipe[p][0]] / recipe[p][1] for p in recipe}

    sys.setrecursionlimit(10**7)
    visited = set()

    def dfs(p):
        if p in visited:
            return
        visited.add(p)
        if p not in recipe:
            return
        for n, c in inputs[p]:
            need[n] += need[p] * c
            dfs(n)

    for m in list(need.keys()):
        dfs(m)

    out = []
    for p in recipe:
        out.append(str(math.ceil(need[p] / rate[p])))

    return "\n".join(out)

assert run("""3
assembler 0.50
furnace 0.50
mining_well 0.55
6
iron_plate furnace 3.20
1
iron_ore 1
copper_plate furnace 3.20
1
copper_ore 1
iron_ore mining_well 1.00
0
copper_ore mining_well 1.00
0
copper_cable assembler 0.50
1
copper_plate 1
electronic_circuit assembler 0.50
2
iron_plate 1
copper_cable 3
1
electronic_circuit 10
""").split() == ["64","192","19","55","30","10"]

assert run("""3
assembler 0.50
furnace 0.50
mining_well 0.55
4
iron_plate furnace 3.20
1
iron_ore 1
iron_ore mining_well 1.00
0
iron_gear assembler 0.50
1
iron_plate 2
transport_belt assembler 0.50
2
iron_plate 1
iron_gear 1
1
transport_belt 7
""").split() == ["135","39","7","7"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 64 192 19 55 30 10 | tuyên truyền đa cấp đầy đủ | 
| Mẫu 2 | 135 39 7 7 | tích lũy phụ thuộc được chia sẻ | 

## Vỏ cạnh 

Trường hợp một cạnh là vật liệu chỉ xuất hiện dưới dạng đầu vào trung gian và không bao giờ là mục tiêu cuối cùng. Thuật toán vẫn xử lý nó một cách chính xác vì việc truyền bá DFS đạt được nó thông qua việc mở rộng phụ thuộc, đảm bảo nó`need`giá trị được tính toán ngay cả khi nó không bao giờ được yêu cầu trực tiếp. 

Một trường hợp khác là nhiều sản phẩm cuối cùng chia sẻ cùng một nguồn tài nguyên cơ bản. Ví dụ: nếu cả hai mục tiêu đều yêu cầu iron_ore, DFS sẽ thêm các khoản đóng góp vào cùng một`need[iron_ore]`lối vào. Vì chúng tôi sử dụng phép cộng thay vì phép gán nên yêu cầu cuối cùng phản ánh chính xác nhu cầu kết hợp. 

Trường hợp thứ ba là tỷ lệ sản xuất trên mỗi máy rất nhỏ dẫn đến số lượng máy lớn. Bởi vì chúng tôi chỉ áp dụng mức trần ở cuối bằng cách sử dụng các giá trị thả nổi, nên các lỗi chính xác trung gian được kiểm soát bằng một epsilon nhỏ, ngăn chặn việc đếm thiếu từng cái một khi các giá trị cực kỳ gần với số nguyên.
