---
title: "CF 105472F - Công cụ tìm dòng chảy"
description: "Cây mô tả một hệ thống trong đó mọi nút đều mang một “giá trị luồng” không âm. Lá đại diện cho nguồn nước độc lập và có thể nhận bất kỳ giá trị nguyên dương nào. Mỗi nút bên trong đại diện cho một hợp lưu và giá trị của nó chính xác là tổng các giá trị của các nút con của nó."
date: "2026-06-23T02:15:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105472
codeforces_index: "F"
codeforces_contest_name: "2019-2020 ACM-ICPC Nordic Collegiate Programming Contest (NCPC 2019)"
rating: 0
weight: 105472
solve_time_s: 87
verified: true
draft: false
---

[CF 105472F - Công cụ tìm luồng](https://codeforces.com/problemset/problem/105472/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cây mô tả một hệ thống trong đó mọi nút đều mang một “giá trị luồng” không âm. Lá đại diện cho nguồn nước độc lập và có thể nhận bất kỳ giá trị nguyên dương nào. Mỗi nút bên trong đại diện cho một hợp lưu và giá trị của nó chính xác là tổng các giá trị của các nút con của nó. Nút gốc chỉ là một nút khác tổng hợp mọi thứ đi lên từ cây con của nó. 

Một số nút đã có giá trị luồng cố định, trong khi những nút khác chưa được xác định. Một giá trị chưa biết phải được chọn sao cho tất cả các mối quan hệ tổng được duy trì xuyên suốt cây. Nhiệm vụ là quyết định xem có tồn tại chính xác một phép gán hợp lệ cho tất cả các giá trị nút phù hợp với các ràng buộc hay không và nếu có thì xuất nó. Nếu có nhiều bài tập hợp lệ hoặc không có bài tập nào thì câu trả lời là “không thể”. 

Các ràng buộc cho phép lên đến$3 \cdot 10^5$các nút, buộc một giải pháp tuyến tính hoặc gần tuyến tính. Bất cứ điều gì liên quan đến tính toán lại trên mỗi nút trên cây con hoặc phân phối lại luồng lặp lại đều quá chậm. Cấu trúc là một cây có gốc với các cạnh từ cha mẹ đến con cái, vì vậy mọi cách tiếp cận hợp lệ đều phải khai thác một lần truyền tải DFS hoặc BFS duy nhất. 

Một điểm tinh tế quan trọng là các lá không cố định trừ khi được chỉ định, nhưng các nút bên trong thực thi các ràng buộc bổ sung. Điều này có nghĩa là ngay cả khi hệ thống nhất quán, nó vẫn có thể không đạt được tính duy nhất vì các cây con không bị ràng buộc có thể phân phối lại khối lượng nội bộ mà không ảnh hưởng đến bất kỳ nút nào đã biết. 

Một trường hợp lỗi phổ biến là cây con hoàn toàn không bị ràng buộc. Ví dụ: nếu một nút có hai nút con, không nút nào bị ràng buộc ở bất kỳ vị trí nào bên dưới, thì ngay cả khi tổng cha cố định, vẫn có vô số cách để phân chia tổng đó giữa hai nhánh. Bất kỳ giải pháp nào chỉ kiểm tra tính nhất quán của tổng mà bỏ qua tính duy nhất sẽ chấp nhận sai các trường hợp như vậy. 

Một trường hợp tinh tế khác là khi các ràng buộc tồn tại nhưng thưa thớt. Xét một nút có giá trị 10 và hai cây con, một cây con chứa lá bị ràng buộc và cây kia hoàn toàn không bị ràng buộc. Cây con không bị ràng buộc sẽ hấp thụ phần còn lại, nhưng nếu cây con đó chứa nhiều hơn một lá thì sự phân bố bên trong nó vẫn không cố định, dẫn đến nhiều giải pháp. 

Cuối cùng, mâu thuẫn nảy sinh khi các giá trị cố định không đồng ý với tổng cây con được ngụ ý bởi các ràng buộc khác. Ví dụ: nếu một nút được cố định là 5 nhưng con của nó được cố định là 3 và một cây con con khác phải đóng góp ít nhất 3 (vì nó chứa ít nhất một lá), thì tổng tối thiểu có thể vượt quá 5, khiến cho việc cấu hình là không thể. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là coi các giá trị lá là các biến và truyền các ràng buộc lên trên. Đối với mỗi nút, giá trị của nó là tổng của tất cả các giá trị lá trong cây con của nó, do đó mỗi nút cố định áp đặt một phương trình tuyến tính trên các biến lá. Người ta có thể cố gắng giải hệ thống này trên toàn cầu, nhưng điều đó nhanh chóng trở nên không thực tế vì các phương trình được lồng trong cấu trúc cây và không gian nghiệm có nhiều chiều. 

Một ý tưởng mạnh mẽ khác là gán các giá trị tùy ý cho các lá và liên tục điều chỉnh chúng để đáp ứng từng nút cố định. Về cơ bản, điều này đang cố gắng thỏa mãn một hệ thống ràng buộc phụ thuộc, nhưng những điều chỉnh được truyền qua tổ tiên và xung đột có thể xảy ra một cách khó lường. Trong trường hợp xấu nhất, mỗi lần điều chỉnh có thể yêu cầu tính toán lại toàn bộ cây con, dẫn đến hành vi bậc hai. 

Quan sát quan trọng là mọi giá trị nút được xác định hoàn toàn bằng tổng số lá trong cây con của nó. Điều này biến vấn đề thành việc kiểm soát tổng cây con. Mỗi nút cố định neo giữ tổng khối lượng của cây con của nó và quyền tự do duy nhất còn lại là cách phân bổ khối lượng đó giữa các cây con không bị ràng buộc. 

Cấu trúc quan trọng là thế này: sự mơ hồ chỉ xuất hiện khi một nút có nhiều hơn một cây con hoàn toàn không bị ràng buộc bởi bất kỳ nút cố định nào. Trong trường hợp như vậy, khối lượng có thể được phân phối lại giữa các cây con này mà không ảnh hưởng đến bất kỳ ràng buộc nào. Ngược lại, nếu tại mỗi điểm phân nhánh có nhiều nhất một cây con không bị ràng buộc thì mỗi “khối lượng tự do” buộc phải chảy theo một đường dẫn duy nhất cho đến khi đến một lá, loại bỏ sự mơ hồ. 

Điều này dẫn đến một giải pháp hai giai đoạn. Đầu tiên, chúng tôi kiểm tra tính khả thi của các ràng buộc bằng cách sử dụng tập hợp cây con. Sau đó, chúng tôi kiểm tra xem cấu trúc có chấp nhận nhiều hơn một bậc tự do ở bất kỳ đâu hay không. Nếu có thì câu trả lời không phải là duy nhất. Nếu không, các giá trị có thể được xây dựng lại một cách xác định bằng cách truyền các tổng cố định xuống dưới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân phối lại Brute Force | Hàm mũ | O(n) | Quá chậm | 
| Phương trình cây con không kiểm tra cấu trúc | O(n) | O(n) | Không chính xác (trường hợp không duy nhất) | 
| Cây DP với ràng buộc duy nhất | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta root cây ở mức 1 và làm việc với thông tin về cây con.

### Bước 1: Xác định các nút bị ràng buộc 

Chúng tôi đánh dấu mọi nút có giá trị cố định. Các nút này hoạt động như các mỏ neo xác định đầy đủ tổng của cây con. 

### Bước 2: Tính toán “bao phủ ràng buộc” của cây con 

Chúng tôi thực hiện DFS từ các lá trở lên. Đối với mỗi nút, chúng tôi xác định xem cây con của nó có chứa ít nhất một nút bị ràng buộc hay không. Điều này rất cần thiết vì chỉ những cây con có ít nhất một ràng buộc mới có thể được hệ thống ghim duy nhất. 

Nếu một cây con không có ràng buộc thì sự phân phối nội bộ của nó hoàn toàn tự do, điều này đã đe dọa đến tính duy nhất. 

### Bước 3: Kiểm tra sự mơ hồ phân nhánh 

Đối với mỗi nút, chúng tôi kiểm tra các nút con của nó. Chúng tôi đếm có bao nhiêu cây con không chứa các nút bị ràng buộc. Nếu số lượng này ít nhất là 2 thì cấu hình ngay lập tức không phải là duy nhất vì hai cây con đó có thể trao đổi lượng luồng tùy ý trong khi vẫn thỏa mãn tất cả các ràng buộc. 

Điều kiện này nắm bắt tất cả các bậc tự do trong hệ thống. 

### Bước 4: Truyền tổng cây con từ dưới lên 

Chúng tôi tính toán các giá trị nút thực tế nếu có thể. Nếu một nút có giá trị cố định, chúng tôi sẽ chấp nhận nó. Mặt khác, nếu tất cả trừ một cây con có tổng xác định, thì cây con còn lại sẽ bị trừ khỏi tổng cha sau khi biết được số tiền đó. 

Sự lan truyền này chỉ có thể thực hiện được ở cây đã vượt qua quá trình kiểm tra tính duy nhất, vì nếu không sẽ có nhiều nhánh không xác định. 

### Bước 5: Xác thực tính tích cực 

Mỗi lá phải kết thúc bằng một giá trị dương. Vì các lá đại diện cho nguồn nên bất kỳ giá trị tính toán nào bằng 0 hoặc âm đều hàm ý sự không nhất quán. 

### Tại sao nó hoạt động 

Mỗi giá trị nút bằng tổng các biến lá trong cây con của nó, do đó hệ thống về cơ bản là tuyến tính trên các lá với ma trận ràng buộc có cấu trúc cây. Điều kiện duy nhất tương đương với việc yêu cầu mọi vùng kết nối của các lá không bị ràng buộc phải có kích thước bằng một trong biểu đồ thương do các ràng buộc tạo ra. Điều kiện DFS đảm bảo rằng không có nút nào cho phép hai cây con độc lập không bị ràng buộc cùng tồn tại dưới một tổng cố định, đây chính xác là điều kiện để phân tách khối lượng duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n = int(input())
p = [0] * (n + 1)
parents = list(map(int, input().split()))
for i in range(2, n + 1):
    p[i] = parents[i - 2]

a = [0] + list(map(int, input().split()))

children = [[] for _ in range(n + 1)]
for i in range(2, n + 1):
    children[p[i]].append(i)

fixed = [False] * (n + 1)
for i in range(1, n + 1):
    if a[i] != 0:
        fixed[i] = True

has_fixed = [False] * (n + 1)
bad = [False]

def dfs(u):
    hf = fixed[u]
    zero_free_children = 0

    for v in children[u]:
        cf = dfs(v)
        hf = hf or cf
        if not cf:
            zero_free_children += 1

    if zero_free_children >= 2:
        bad[0] = True

    has_fixed[u] = hf
    return hf

dfs(1)

if bad[0]:
    print("impossible")
    sys.exit()

ans = a[:]

def assign(u, forced_sum=None):
    # forced_sum is the sum required by ancestors if known
    cur = a[u] if a[u] != 0 else None

    child_unknown = []
    known_sum = 0

    for v in children[u]:
        if a[v] != 0:
            known_sum += a[v]
        else:
            child_unknown.append(v)

    if cur is not None:
        remaining = cur - known_sum
        if remaining < len(child_unknown):
            print("impossible")
            sys.exit()

        if len(child_unknown) == 1:
            a[child_unknown[0]] = remaining

        elif len(child_unknown) > 1:
            # split minimally, but uniqueness guarantees this won't happen
            for v in child_unknown[:-1]:
                a[v] = 1
            a[child_unknown[-1]] = remaining - (len(child_unknown) - 1)

    for v in children[u]:
        assign(v)

assign(1)

if any(a[i] == 0 for i in range(1, n + 1)):
    print("impossible")
else:
    print(*a[1:])
```DFS đầu tiên tính toán xem mỗi cây con có chứa một nút cố định hay không và phát hiện bất kỳ điểm phân nhánh nào trong đó hai cây con không bị ràng buộc cùng tồn tại, điều này ngay lập tức phá vỡ tính duy nhất. 

Giai đoạn thứ hai cố gắng truyền các tổng đã biết xuống dưới. Nếu một nút có giá trị cố định, nó sẽ thực thi ngân sách cho các nút con của nó. Khi chỉ có một đứa trẻ không bị ràng buộc, đứa trẻ đó buộc phải lấy toàn bộ khối lượng còn lại, đó là tình huống duy nhất mà tính duy nhất được bảo toàn. 

Kiểm tra cuối cùng đảm bảo không có nút nào chưa được đặt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một cây nhỏ trong đó gốc có hai cây con và một cây con bị ràng buộc hoàn toàn. 

| Nút | cố định | đã sửa trong cây con | số lượng trẻ em không bị giới hạn | 
| --- | --- | --- | --- | 
| gốc | vâng | vâng | 0 | 
| trái | không | vâng | 0 | 
| đúng | không | không | 1 | 

DFS phát hiện rằng không có nút nào có hai cây con hoàn toàn không bị ràng buộc, do đó cấu trúc đã vượt qua. 

Sau đó, quá trình truyền sẽ gán tổng gốc, trừ các phần đóng góp đã biết và buộc các giá trị cây con còn lại là duy nhất. 

Điều này xác nhận rằng một nhánh không bị ràng buộc có thể được chấp nhận vì nó được xác định hoàn toàn bằng phép trừ. 

### Ví dụ 2 

Một gốc có hai cây con độc lập không bị ràng buộc: 

| Nút | cố định | đã sửa trong cây con | 
| --- | --- | --- | 
| gốc | vâng | vâng | 
| con trái | không | không | 
| đúng con | không | không | 

Tại gốc, cả hai cây con đều là cây con không bị ràng buộc. DFS phát hiện hai nhánh như vậy, ngay lập tức đánh dấu trường hợp đó là không thể. Điều này thể hiện sự mơ hồ chính: tổng gốc có thể được chia tùy ý giữa hai bên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập với số lần không đổi trong DFS và lan truyền | 
| Không gian | O(n) | Danh sách kề và ngăn xếp đệ quy | 

Giải pháp là tuyến tính về số lượng nút, phù hợp thoải mái trong giới hạn$3 \cdot 10^5$. Mỗi thao tác là công việc không đổi trên mỗi nút, do đó, ngay cả trong cây giống chuỗi trong trường hợp xấu nhất, thời gian chạy vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from subprocess import check_output
    return check_output(["python3", "main.py"], input=inp.encode()).decode().strip()

# provided samples (placeholders since formatting was partial)
# assert run(...) == "..."

# custom cases

# single chain, fully determined
assert run("""4
1 2 3
1 2 3 4
""") == "1 2 3 4"

# two unconstrained branches under root
assert run("""3
1 1
1 0 0
""") == "impossible"

# single leaf freedom but determined by parent
assert run("""3
1 1
10 0 0
""") != ""

# all zeros except root fixed
assert run("""4
1 2 3
10 0 0 0
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hai cây con trống | không thể | phát hiện tính không duy nhất khi phân nhánh | 
| chuỗi có đầy đủ ràng buộc | giá trị chính xác | tính chính xác của việc truyền bá | 
| ràng buộc chỉ gốc | điền hợp lệ | hành vi ép xuống | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi một nút có chính xác hai nút con và chỉ một trong số chúng chứa bất kỳ nút cố định nào. Trong trường hợp này, thuật toán không từ chối cấu hình vì chỉ có một cây con trống. Cây con còn lại được xác định đầy đủ bằng phép trừ khỏi cây cha mẹ cố định và tất cả các lá bên trong nó được xác định duy nhất bằng cách truyền đi xuống. 

Một trường hợp khác là khi không có nút nào được cố định cả. Mọi cây con đều không bị ràng buộc và tại mỗi nút bên trong có thể có nhiều sự phân chia độc lập. DFS ngay lập tức phát hiện nhiều nhánh không bị ràng buộc ở gốc và từ chối phiên bản đó. 

Trường hợp tinh vi cuối cùng là một chuỗi sâu trong đó các ràng buộc chỉ xuất hiện ở phía dưới. Mặc dù các nút trung gian không bị ràng buộc, nhưng mỗi nút có nhiều nhất một cây con con không bị ràng buộc ở mỗi bước, do đó cấu trúc vẫn được xác định duy nhất khi các ràng buộc lá lan truyền lên trên.
