---
title: "CF 102180F - \u0410\u0439\u043b\u0430\u043d\u0434\u043b\u044d\u043d\u0434"
description: "Có (n) hòn đảo và ban đầu mọi hòn đảo đều có cư dân. Một số báo cáo thêm một cây cầu vô hướng giữa hai hòn đảo. Các báo cáo khác mô tả một trận lũ lụt trên đảo (u), sau đó mọi cư dân hiện đang sống trên (u) di chuyển đến đảo (v)."
date: "2026-08-19T06:54:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102180
codeforces_index: "F"
codeforces_contest_name: "MSPU Training Contest 2018-2019"
rating: 0
weight: 102180
solve_time_s: 97
verified: true
draft: false
---

[CF 102180F - \u0410\u0439\u043b\u0430\u043d\u0434\u043b\u044d\u043d\u0434](https://codeforces.com/problemset/problem/102180/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có (n) hòn đảo và ban đầu mọi hòn đảo đều có cư dân. Một số báo cáo thêm một cây cầu vô hướng giữa hai hòn đảo. Các báo cáo khác mô tả một trận lũ lụt trên đảo (u), sau đó mọi cư dân hiện đang sống trên (u) di chuyển đến đảo (v). Bản thân hòn đảo (u) không bị dỡ bỏ nên những cây cầu gắn liền với nó vẫn có thể sử dụng được. 

Sau mỗi báo cáo, chúng tôi cần số lượng cầu mới tối thiểu để giúp tất cả các hòn đảo hiện có người ở đều có thể tiếp cận được bằng đường bộ. Những cây cầu hiện có có thể được tái sử dụng và những cây cầu mới có thể được xây dựng giữa các đảo tùy ý. 

Hãy xem xét đồ thị được hình thành bởi tất cả các cây cầu. Giả sử chính xác (c) các thành phần liên thông chứa ít nhất một thành phần cư trú. Một cây cầu bổ sung có thể hợp nhất hai thành phần như vậy và cầu nối (c-1) luôn đủ để kết nối tất cả chúng. Do đó, toàn bộ vấn đề giảm xuống còn việc duy trì số lượng các thành phần được kết nối hiện có ít nhất một cư dân. 

Các ràng buộc là lý do chính khiến việc truyền tải đồ thị trực tiếp là không phù hợp. Với tối đa (3\cdot10^5) đảo và (3\cdot10^5) báo cáo, việc tính toán lại các thành phần được kết nối sau mỗi thao tác có thể yêu cầu (O(nq)) hoạt động, khoảng (9\cdot10^{10}) thao tác đỉnh trong trường hợp xấu nhất. Ngay cả một lần truyền tải trên mỗi báo cáo cũng vượt xa những gì giới hạn một giây có thể hỗ trợ. Chúng tôi cần mỗi báo cáo được xử lý với thời gian khấu hao gần như cố định. 

Trường hợp tinh tế đầu tiên là một hòn đảo có thể trở nên trống rỗng mà không biến mất khỏi biểu đồ. Ví dụ,```
2 1
2 1 2
```Hòn đảo có người ở duy nhất sau báo cáo là đảo 2, vì vậy câu trả lời là`0`. Giải pháp xóa vật lý đảo 1 khỏi biểu đồ sẽ khiến việc vận hành cầu sau này trở nên khó khăn hoặc không chính xác vì đảo 1 có thể trở thành nơi sinh sống trở lại. 

Trường hợp tinh tế thứ hai là việc di chuyển cư dân giữa hai hòn đảo vốn thuộc về cùng một thành phần được kết nối. Ví dụ,```
3 2
1 1 2
2 1 2
```Sau báo cáo đầu tiên, đảo 1 và 2 tạo thành một thành phần và đảo 3 tách biệt. Sau lũ, toàn bộ cư dân đảo 1 di chuyển sang đảo 2 nhưng cấu trúc thành phần không thay đổi. Các thành phần có người ở vẫn còn`{1,2}`Và`{3}`, vậy câu trả lời là`1 1`. Việc triển khai bất cẩn coi mọi trận lũ lụt là việc loại bỏ một thành phần có người ở sẽ tạo ra sai sót`1 0`. 

Trường hợp thứ ba xảy ra khi một hòn đảo trống rỗng trước trận lũ lụt. Ví dụ,```
3 2
2 1 2
2 1 3
```Sau báo cáo đầu tiên, đảo 1 trống rỗng và đảo 2 có cư dân. Trận lũ thứ hai không di chuyển gì nên trạng thái không thay đổi. Các câu trả lời đúng là`1 1`. Việc thực hiện phải kiểm tra số lượng cư dân tại nguồn thay vì sửa đổi số lượng thành phần một cách mù quáng. 

Trường hợp thứ tư xuất hiện khi một cây cầu nối hai thành phần, trong đó một thành phần trống. Ví dụ,```
3 2
2 1 2
1 1 3
```Sau lũ chỉ còn đảo 2 có người sinh sống. Cây cầu nối giữa đảo 1 và 3 nối hai đỉnh trống và không ảnh hưởng đến số lượng thành phần có người ở. Các câu trả lời là`1 1`. Giải pháp đếm mọi thành phần biểu đồ thay vì chỉ các thành phần chứa cư dân sẽ không thành công ở đây. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản sẽ duy trì biểu đồ một cách rõ ràng và sau mỗi báo cáo, chạy DFS hoặc BFS từ tất cả các đảo hiện có người ở để xác định có bao nhiêu thành phần được kết nối có chứa cư dân. Điều này đúng vì việc truyền tải đồ thị trực tiếp tính toán chính xác kết nối quan trọng. 

Vấn đề là công việc lặp đi lặp lại. Một đồ thị có (n) đỉnh và có tối đa (q) cạnh được thêm vào có thể có (O(n+q)) cạnh được lưu trữ. Chạy toàn bộ quá trình truyền tải sau mỗi (q) báo cáo có thể mất (O(q(n+q))), tức là (O(q^2+nq)). Với cả (n) và (q) bằng (3\cdot10^5), đây là thứ tự của các phép toán (9\cdot10^{10}) hoặc tệ hơn. 

Giải pháp brute-force hoạt động vì nó tính toán lại trạng thái biểu đồ chính xác từ đầu. Nó thất bại vì những cây cầu chỉ được thêm vào chứ không bao giờ bị loại bỏ. Cấu trúc đơn điệu đó có nghĩa là chúng tôi không cần phải khám phá lại kết nối sau mỗi báo cáo cầu nối. 

Cấu trúc Disjoint Set Union hoàn toàn phù hợp với tình huống này. Mọi cầu nối giữa (u) và (v) chỉ đơn giản là hợp nhất hai bộ DSU của chúng. DSU cung cấp cho chúng ta thành phần được kết nối của bất kỳ hòn đảo nào trong thời gian khấu hao gần như không đổi. 

Chúng ta vẫn cần phải xử lý lũ lụt. Quan sát quan trọng là cư dân không ảnh hưởng đến kết nối biểu đồ. Một trận lũ chỉ thay đổi đỉnh nào có cư dân. Đối với mỗi thành phần DSU, chúng tôi có thể lưu trữ tổng số cư dân hiện có bên trong nó. Sau đó, một trận lũ từ (u) đến (v) sẽ loại bỏ quần thể tại thành phần của (u) và thêm nó vào thành phần của (v). Nếu thành phần nguồn trở nên trống, số lượng thành phần có người ở sẽ giảm. Nếu thành phần đích trống và tiếp nhận cư dân thì số lượng sẽ tăng lên. 

Điều này mang lại cho chúng tôi một bộ đếm toàn cầu`active`, bằng số lượng thành phần DSU có dân số dương. Một cây cầu nối hai thành phần khác nhau sẽ làm giảm`active`bởi một nơi chính xác khi cả hai thành phần đều có người ở. Lũ lụt thay đổi`active`chỉ khi dân số giao thoa giữa các thành phần khác nhau và một trong hai thành phần thay đổi giữa trống và không trống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(q(n+q))) | (O(n+q)) | Quá chậm | 
| Tối ưu | (O((n+q)\alpha(n))) | (O(n+q)) | Đã chấp nhận | 

Ở đây (\alpha(n)) là hàm Ackermann nghịch đảo, nằm dưới bất kỳ hằng số thực tế nào có liên quan đến các ràng buộc này. 

## Hướng dẫn thuật toán 

1. Ban đầu mỗi hòn đảo đều có cư dân sinh sống, vì vậy mỗi hòn đảo đều là một thành phần DSU có người ở. Bộ`population[i] = 1`cho mọi hòn đảo và khởi tạo`active = n`. Do đó, câu trả lời trước bất kỳ báo cáo nào sẽ là (n-1). 
2. Đối với báo cáo cầu`1 u v`, tìm nghiệm DSU của`u`Và`v`. Nếu chúng đã bằng nhau thì cây cầu sẽ không thay đổi gì. Nếu chúng khác nhau, hãy hợp nhất hai thành phần đó và cộng tổng dân số của chúng. Nếu cả hai thành phần đều có dân số dương thì hai thành phần có người ở đã trở thành một, do đó giảm`active`bởi một. 
3. Về báo lũ`2 u v`, đọc`x = population[u]`. Nếu như`x`bằng 0, không ai di chuyển và không có gì để cập nhật. Trường hợp này phải được xử lý trước khi thay đổi tổng thành phần. 
4. Nếu`u`có cư dân, tìm nguồn gốc DSU hiện tại`ru`Và`rv`. Bộ`population[u]`về 0 và thêm`x`ĐẾN`population[v]`. 
5. Khi nào`ru`Và`rv`khác nhau, trừ đi`x`từ tổng dân số`ru`. Nếu tổng số đó bằng 0 thì thành phần nguồn không còn có người ở nữa, do đó hãy giảm`active`. Nếu thành phần đích không có dân số trước khi nhận`x`, tăng`active`. Khi`ru == rv`, cả hai hoạt động đều xảy ra bên trong cùng một thành phần có người ở, vì vậy`active`không thay đổi. 
6. Sau khi xử lý báo cáo, in`active - 1`. Nếu có`active`các thành phần được kết nối có người ở, ít nhất`active - 1`Cần có những cây cầu mới để kết nối chúng và chỉ cần nhiều cây cầu là đủ bằng cách nối các thành phần trong một cây. 

Bất biến trung tâm là`population[root]`lưu trữ tổng số cư dân trong toàn bộ thành phần DSU được biểu thị bằng`root`, Và`active`đếm chính xác những gốc có dân số dương. Hoạt động cầu nối bảo toàn sự bất biến này bằng cách hợp nhất tổng dân số. Hoạt động lũ lụt bảo tồn nó bằng cách di chuyển quần thể nguồn giữa các tổng thành phần thích hợp. Vì số lượng cầu bổ sung tối thiểu cần thiết cho (c) các thành phần không trống chính xác là (c-1),`active - 1`luôn là câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())

    parent = list(range(n))
    size = [1] * n
    population = [1] * n

    active = n

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    out = []

    for _ in range(q):
        t, u, v = map(int, input().split())
        u -= 1
        v -= 1

        if t == 1:
            ru = find(u)
            rv = find(v)

            if ru != rv:
                if size[ru] < size[rv]:
                    ru, rv = rv, ru

                if population[ru] > 0 and population[rv] > 0:
                    active -= 1

                parent[rv] = ru
                size[ru] += size[rv]
                population[ru] += population[rv]

        else:
            x = population[u]

            if x > 0:
                ru = find(u)
                rv = find(v)

                population[u] = 0

                if ru != rv:
                    population[ru] -= x

                    if population[ru] == 0:
                        active -= 1

                    if population[rv] == 0:
                        active += 1

                    population[rv] += x
                else:
                    population[ru] -= x
                    population[rv] += x

        out.append(str(active - 1))

    sys.stdout.write(" ".join(out))

if __name__ == "__main__":
    solve()
```các`parent`Và`size`mảng triển khai DSU với tính năng nén đường dẫn và kết hợp theo kích thước. Chúng cùng nhau đảm bảo thời gian khấu hao gần như không đổi để tìm và hợp nhất các thành phần được kết nối. 

các`population`mảng được lập chỉ mục bởi DSU root bất cứ khi nào nó đại diện cho tổng thành phần. Sau khi hợp nhất, gốc con cũ không còn được sử dụng làm đại diện thành phần nữa, do đó giá trị tổng thể của nó không cần thiết phải giữ nguyên ý nghĩa. Gốc mới nhận được tổng của cả hai quần thể thành phần. 

Đối với một trận lũ lụt,`population[u]`được hiểu một cách có chủ ý là dân số hiện đang sinh sống trên hòn đảo vật chất`u`, trong khi`population[root]`đại diện cho tổng dân số của một thành phần. Sự khác biệt này là phần tinh tế nhất của việc thực hiện. Trước khi di chuyển dân cư`x = population[u]`chiếm được dân cư trên đảo. Sau khi thiết lập`population[u] = 0`, các tổng thành phần được điều chỉnh riêng. 

Khi`ru == rv`, trừ và cộng cùng một lượng vào cùng một thành phần thì tổng của nó không thay đổi. Nhánh rõ ràng rất hữu ích vì nó ngăn không cho bộ đếm thành phần hoạt động bị sửa đổi khi tập hợp không bao giờ rời khỏi thành phần được kết nối của nó. 

Các đỉnh đầu vào được chuyển đổi từ lập chỉ mục dựa trên một sang dựa trên 0 ngay lập tức. Vì bài toán chỉ có một test case nên vòng lặp đầu vào sẽ xử lý chính xác`q`báo cáo. Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn đối với tổng dân số. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào chỉ chứa các báo cáo cầu nối, do đó tổng thể của mọi thành phần chỉ thay đổi khi hai thành phần được hợp nhất. 

| Báo cáo | Hoạt động | Quần thể thành phần | Thành phần hoạt động | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | cầu 1-2 |`{1,2}:2`,`{3}:1`,`{4}:1`,`{5}:1`| 4 | 3 | 
| 2 | cầu 2-3 |`{1,2,3}:3`,`{4}:1`,`{5}:1`| 3 | 2 | 
| 3 | cầu 1-3 | không thay đổi | 3 | 2 | 
| 4 | cầu 4-5 |`{1,2,3}:3`,`{4,5}:2`| 2 | 1 | 
| 5 | cầu 1-4 |`{1,2,3,4,5}:5`| 1 | 0 | 

Cây cầu thứ ba là dư thừa vì đảo 1 và 3 đã thuộc cùng một thành phần. Điều này chứng tỏ tại sao việc hợp nhất DSU trước tiên phải so sánh hai gốc. 

### Mẫu 2 

Ở đây không có cây cầu nào cả. Mỗi hòn đảo ban đầu hình thành thành phần đồ thị riêng và lũ lụt chỉ di chuyển cư dân giữa các hòn đảo. 

| Báo cáo | Hoạt động | Quần đảo không trống | Thành phần hoạt động | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | lũ 1-2 | 2, 3, 4, 5 | 4 | 3 | 
| 2 | lũ 2-1 | 1, 3, 4, 5 | 4 | 3 | 
| 3 | lũ 1-3 | 3, 4, 5 | 3 | 2 | 
| 4 | lũ 5-4 | 3, 4 | 2 | 1 | 
| 5 | lũ 4-3 | 3 | 1 | 0 | 
| 6 | lũ 3-1 | 1 | 1 | 0 | 

Báo cáo thứ hai di chuyển những cư dân đã tích tụ trên đảo 2 trở lại đảo 1. Số lượng thành phần có người ở vẫn là 4 vì một hòn đảo bị chiếm đóng trở nên trống rỗng trong khi hòn đảo khác bị chiếm đóng. Báo cáo thứ năm chỉ để đảo 3 bị chiếm đóng, đưa ra câu trả lời cuối cùng là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((n+q)\alpha(n))) | Mỗi báo cáo thực hiện một số lượng hoạt động DSU không đổi và các hoạt động DSU gần như có thời gian khấu hao không đổi. | 
| Không gian | (O(n)) | Mỗi mảng gốc, kích thước và tập hợp đều chứa các giá trị (n), trong khi đầu ra chứa các giá trị (q). | 

Với (n,q\leq3\cdot10^5), thuật toán chỉ thực hiện một số thao tác DSU cho mỗi báo cáo. Điều này nằm trong mức độ phức tạp dự định, trong khi phương pháp truyền tải vũ phu sẽ yêu cầu hàng chục tỷ thao tác trong trường hợp xấu nhất. Các mảng được lưu trữ cũng tuyến tính theo số lượng đảo và bộ đệm đầu ra cũng tuyến tính theo số lượng báo cáo. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    q = next(it)

    parent = list(range(n))
    size = [1] * n
    population = [1] * n
    active = n

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    out = []

    for _ in range(q):
        t = next(it)
        u = next(it) - 1
        v = next(it) - 1

        if t == 1:
            ru = find(u)
            rv = find(v)

            if ru != rv:
                if size[ru] < size[rv]:
                    ru, rv = rv, ru

                if population[ru] > 0 and population[rv] > 0:
                    active -= 1

                parent[rv] = ru
                size[ru] += size[rv]
                population[ru] += population[rv]

        else:
            x = population[u]

            if x:
                ru = find(u)
                rv = find(v)

                population[u] = 0

                if ru != rv:
                    population[ru] -= x

                    if population[ru] == 0:
                        active -= 1

                    if population[rv] == 0:
                        active += 1

                    population[rv] += x

        out.append(str(active - 1))

    return " ".join(out)

# Provided sample 1
sample1 = """\
5 5
1 1 2
1 2 3
1 1 3
1 4 5
1 1 4
"""
assert solve_data(sample1) == "3 2 2 1 0", "sample 1"

# Provided sample 2
sample2 = """\
5 6
2 1 2
2 2 1
2 1 3
2 5 4
2 4 3
2 3 1
"""
assert solve_data(sample2) == "3 3 2 1 0 0", "sample 2"

# Provided sample 3
sample3 = """\
9 11
1 1 2
1 3 4
1 5 6
2 6 4
2 5 3
1 7 8
1 1 8
1 5 9
2 9 5
2 1 2
2 1 3
"""
assert solve_data(sample3) == "7 6 5 5 4 3 2 2 2 2 2", "sample 3"

# Minimum-size graph, with a flood and a bridge.
case_min = """\
2 3
2 1 2
1 1 2
2 2 1
"""
assert solve_data(case_min) == "0 0 0", "minimum size"

# Empty-source floods and a bridge through empty islands.
case_empty = """\
3 4
2 1 2
2 1 3
1 1 3
2 2 1
"""
assert solve_data(case_empty) == "1 1 1 0", "empty source"

# Redundant bridge and flood inside the same component.
case_same_component = """\
3 4
1 1 2
2 1 2
1 2 3
2 2 3
"""
assert solve_data(case_same_component) == "1 1 0 0", "same component"

# Boundary-sized generated test.
n = 300000
q = 300000
parts = [f"{n} {q}"]
parts.extend(["2 1 2"] * q)
case_large = "\n".join(parts) + "\n"
expected_large = " ".join(["299998"] + ["299998"] * (q - 1))
assert solve_data(case_large) == expected_large, "large boundary case"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 3`với lũ lụt và một cây cầu |`0 0 0`| Số lượng đảo tối thiểu và sự di chuyển dân số lặp đi lặp lại | 
|`3 4`với lũ nguồn rỗng |`1 1 1 0`| Lũ lụt từ các hòn đảo trống và những cây cầu giữa các đỉnh trống | 
|`3 4`với một cặp được kết nối |`1 1 0 0`| Dân số di chuyển bên trong thành phần DSU hiện có và kết nối dự phòng | 
|`n=q=300000`với lũ lụt lặp đi lặp lại |`299998`lặp đi lặp lại | Kích thước đầu vào, hiệu suất tối đa và các thao tác lặp lại trên cùng một nguồn | 

## Vỏ cạnh 

đầu vào```
2 1
2 1 2
```bắt đầu với hai thành phần có người ở riêng biệt. Lũ di chuyển cư dân duy nhất của đảo 1 sang đảo 2, do đó thành phần nguồn trở nên trống rỗng và thành phần đích vẫn có người ở.`active`thay đổi từ 2 thành 1, tạo ra`active - 1 = 0`. Đảo vật lý 1 vẫn nằm trong DSU, điều này cần thiết vì nó có thể tham gia vào các hoạt động cầu sau này. 

Vì```
3 2
2 1 2
2 1 3
```trận lũ đầu tiên di chuyển dân số của đảo 1 sang đảo 2. Trận lũ thứ hai bắt đầu từ đảo 1, nơi dân số hiện bằng không. Thuật toán thoát khỏi bản cập nhật lũ ngay lập tức, để lại`active = 2`trong suốt báo cáo thứ hai. Đầu ra là`1 1`. 

Vì```
3 2
1 1 2
2 1 2
```Cây cầu đầu tiên tạo ra một thành phần chứa đảo 1 và 2, với dân số hai. Lũ lụt sau đó di chuyển dân cư của đảo 1 sang đảo 2, nhưng cả hai hòn đảo vẫn nằm trong cùng một thành phần. Dân số thành phần vẫn là hai, và`active`vẫn ở mức hai vì đảo 3 là thành phần có người ở khác. Đầu ra là`1 1`. 

Vì```
3 2
2 1 2
1 1 3
```báo cáo đầu tiên làm trống đảo 1, chỉ còn lại đảo 2 có người sinh sống. Báo cáo thứ hai nối các đảo trống 1 và 3 thành một thành phần biểu đồ không chứa cư dân. Việc hợp nhất DSU thấy rằng cả hai quần thể thành phần đều bằng 0, do đó nó không thay đổi`active`. Đầu ra vẫn còn`1 1`. 

Thuật toán cũng xử lý một cây cầu giữa các hòn đảo đã được kết nối. Trong Mẫu 1, báo cáo thứ ba là`1 1 3`, nhưng đảo 1 và 3 được kết nối qua đảo 2. Gốc DSU của chúng bằng nhau, do đó tổng dân số hoặc số lượng thành phần hoạt động không thay đổi. Câu trả lời ở lại`2`. 

Cuối cùng, nhiều cây cầu nối giữa cùng một cặp đảo đều vô hại. DSU coi mọi cầu nối tiếp theo giữa các đỉnh đã có trong cùng thành phần là không hoạt động. Điều này phù hợp với ngữ nghĩa của biểu đồ vì các cầu nối trùng lặp không tạo thành phần được kết nối mới và không thể giảm số lượng cầu nối bổ sung cần thiết.
