---
title: "CF 277A - Học ngôn ngữ"
description: "Chúng tôi được yêu cầu đảm bảo rằng mọi nhân viên tại BerCorp đều có thể giao tiếp với mọi nhân viên khác, trực tiếp thông qua ngôn ngữ chung hoặc gián tiếp thông qua một chuỗi phiên dịch."
date: "2026-06-05T05:48:16+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu"]
categories: ["algorithms"]
codeforces_contest: 277
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 170 (Div. 1)"
rating: 1400
weight: 277
solve_time_s: 94
verified: true
draft: false
---

[CF 277A - Học ngôn ngữ](https://codeforces.com/problemset/problem/277/A) 

**Đánh giá:** 1400 
**Thẻ:** dfs và tương tự, dsu 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đảm bảo rằng mọi nhân viên tại BerCorp đều có thể giao tiếp với mọi nhân viên khác, trực tiếp thông qua ngôn ngữ chung hoặc gián tiếp thông qua một chuỗi phiên dịch. Mỗi nhân viên ban đầu có thể không biết hoặc nhiều ngôn ngữ chính thức và chúng tôi có thể trả tiền để họ học các ngôn ngữ bổ sung với chi phí là 1 ngôn ngữ cho mỗi nhân viên cho mỗi ngôn ngữ. Mục tiêu là giảm thiểu tổng chi phí cần thiết để kết nối tất cả nhân viên theo cách này. 

Từ đầu vào, chúng ta có`n`nhân viên và`m`ngôn ngữ. Danh sách của mỗi nhân viên chứa các ngôn ngữ họ biết. Đầu ra là một số nguyên duy nhất: số lượng bài học trả phí tối thiểu để tất cả nhân viên có thể tiếp cận được với nhau. Những nhân viên không biết ngôn ngữ ban đầu sẽ bị ngắt kết nối và sẽ luôn cần ít nhất một bài học để tham gia bất kỳ mạng liên lạc nào. 

Những hạn chế ở mức vừa phải: cả hai`n`Và`m`nhiều nhất là 100, vậy thậm chí`O(n*m)`hoạt động là khả thi. Tuy nhiên, việc cố gắng hết sức ngây thơ để thử mọi sự kết hợp giữa những người học ngôn ngữ nào là theo cấp số nhân và không thể thực hiện được. Các trường hợp đặc biệt bao gồm tất cả nhân viên không biết ngôn ngữ, trong trường hợp đó mỗi nhân viên bị cô lập và chi phí tối thiểu bằng số lượng nhân viên hoặc tất cả nhân viên đã biết chuỗi ngôn ngữ, không mất chi phí. Một trường hợp phức tạp khác là khi tồn tại nhiều nhóm bị ngắt kết nối; chỉ cần dạy một ngôn ngữ cho một người trong mỗi nhóm có thể kết nối họ một cách hiệu quả. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các nhóm bài tập ngôn ngữ có thể có cho những nhân viên không biết gì, tính toán kết nối thu được và chọn chi phí tối thiểu. Điều này đúng về nguyên tắc nhưng rõ ràng là không khả thi, vì ngay cả đối với`n=100`nó sẽ yêu cầu kiểm tra`m^n`sự kết hợp. 

Cái nhìn sâu sắc quan trọng là đây là một vấn đề kết nối. Nếu chúng ta coi nhân viên là các nút trong biểu đồ và kết nối hai nhân viên nếu họ dùng chung một ngôn ngữ thì vấn đề sẽ giảm xuống việc đếm các thành phần được kết nối trong biểu đồ ngôn ngữ nhân viên này. Nhân viên trong cùng một thành phần đã có thể giao tiếp nên chi phí chỉ phát sinh khi kết nối các thành phần khác nhau. Để hợp nhất`k`thành phần, chúng ta cần ít nhất`k-1`những kết nối mới. Mỗi kết nối có thể đạt được bằng cách dạy cho bất kỳ nhân viên nào trong một thành phần một ngôn ngữ từ thành phần khác. 

Một sự tinh tế nữa là xử lý trường hợp đặc biệt khi không nhân viên nào biết bất kỳ ngôn ngữ nào. Trong trường hợp đó, mỗi nhân viên ban đầu là một thành phần biệt lập. Vì chúng tôi không thể dựa vào sự chồng chéo ngôn ngữ hiện có nên chúng tôi cần dạy mỗi nhân viên ít nhất một ngôn ngữ, dẫn đến chi phí tương đương`n`. 

Cách tiếp cận tối ưu sử dụng cấu trúc tập hợp rời rạc (DSU). Chúng tôi lặp lại các ngôn ngữ và tập hợp tất cả nhân viên biết ngôn ngữ đó. Cuối cùng, số lượng bộ DSU riêng biệt cho biết số lượng nhóm bị ngắt kết nối ban đầu. Nếu ít nhất một nhân viên biết một ngôn ngữ thì số bài học tối thiểu cần có là số nhóm trừ đi một. Nếu không có nhân viên nào biết ngoại ngữ thì số bài học tối thiểu bằng`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m^n) | O(n*m) | Quá chậm | 
| DSU / Các thành phần được kết nối | O(n*m + n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo DSU với`n`các yếu tố, một cho mỗi nhân viên. Mỗi nhân viên bắt đầu trong bộ riêng của họ. Điều này mô hình hóa ý tưởng ban đầu, mỗi nhân viên có thể bị cô lập. 
2. Theo dõi cờ boolean`any_language_known`để phát hiện xem có ít nhất một nhân viên biết một ngôn ngữ hay không. 
3. Lặp lại từng nhân viên và danh sách các ngôn ngữ đã biết của họ. Đối với mỗi ngôn ngữ, hãy duy trì một danh sách nhân viên biết ngôn ngữ đó. Đây là một ánh xạ ngược từ ngôn ngữ đến nhân viên. 
4. Đối với mỗi ngôn ngữ, nếu có từ hai nhân viên trở lên biết thì kết hợp tất cả họ vào DSU. Bước này hợp nhất những nhân viên đã có thể giao tiếp qua ngôn ngữ dùng chung thành một thành phần duy nhất. 
5. Đếm số lượng các thành phần được kết nối riêng biệt bằng cách kiểm tra gốc của từng nhân viên trong DSU. Nếu như`any_language_known`là Đúng, số lượng bài học tối thiểu là`components - 1`. Nếu không có nhân viên nào biết ngoại ngữ thì số buổi học tối thiểu bằng`n`. 

Tại sao nó hoạt động: DSU duy trì các thành phần được kết nối một cách chính xác khi các nhân viên công đoàn của chúng tôi chia sẻ một ngôn ngữ. Hoạt động công đoàn đảm bảo rằng một khi hai nhân viên được kết nối, tất cả các công đoàn trong tương lai sẽ duy trì kết nối. Việc đếm trực tiếp số lượng thành phần cuối cùng sẽ đưa ra số lượng kết nối (bài học) bổ sung cần thiết để hợp nhất tất cả các nhóm, thu được chi phí tối thiểu. Trường hợp biên của ngôn ngữ 0 được xử lý riêng vì không có sự kết hợp nào xảy ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px != py:
            self.parent[py] = px

def main():
    n, m = map(int, input().split())
    dsu = DSU(n)
    lang_to_employees = [[] for _ in range(m)]
    any_language_known = False
    
    for i in range(n):
        data = list(map(int, input().split()))
        k = data[0]
        if k > 0:
            any_language_known = True
        for lang in data[1:]:
            lang_to_employees[lang - 1].append(i)
    
    for employees in lang_to_employees:
        for i in range(1, len(employees)):
            dsu.union(employees[0], employees[i])
    
    roots = set(dsu.find(i) for i in range(n))
    if any_language_known:
        print(len(roots) - 1)
    else:
        print(n)

if __name__ == "__main__":
    main()
```Mã này tạo ra một DSU cho nhân viên và sử dụng ánh xạ từ ngôn ngữ đến nhân viên để hợp nhất một cách hiệu quả tất cả nhân viên chia sẻ một ngôn ngữ. Lá cờ`any_language_known`xử lý trường hợp cạnh của kiến ​​​​thức ban đầu bằng không. Chúng tôi trừ đi một từ số thành phần được kết nối vì việc hợp nhất`k`các nhóm yêu cầu`k-1`bài học mới. 

## Ví dụ đã hoạt động 

Mẫu 1:```
5 5
1 2
2 2 3
2 3 4
2 4 5
1 5
```| Nhân viên | Ngôn ngữ | Root DSU sau khi xử lý | 
| --- | --- | --- | 
| 1 | 2 | 0 | 
| 2 | 2,3 | 0 | 
| 3 | 3,4 | 0 | 
| 4 | 4,5 | 0 | 
| 5 | 5 | 0 | 

Tất cả nhân viên chia sẻ một chuỗi ngôn ngữ, DSU hợp nhất mọi người thành một nhóm.`roots = {0}`. Vì biết ít nhất một ngôn ngữ nên chi phí là`1-1=0`. 

Mẫu 2 (tất cả nhân viên bị cách ly):```
3 2
0
0
0
```Không có nhân viên nào biết bất kỳ ngôn ngữ nào.`roots = {0,1,2}`.`any_language_known = False`, vậy chi phí là`n = 3`. 

Những dấu vết này cho thấy thuật toán xử lý chính xác cả trường hợp tiêu chuẩn và trường hợp biên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m) | Ngôn ngữ của mỗi nhân viên được xử lý một lần và liên kết/tìm kiếm DSU có thời gian khấu hao gần như không đổi do nén đường dẫn. | 
| Không gian | O(n + m) | DSU sử dụng O(n) và ánh xạ ngôn ngữ tới nhân viên sử dụng O(m + tổng số ngôn ngữ) ≤ O(n*m). | 

Với những hạn chế`n,m ≤ 100`, số lượng hoạt động trong trường hợp xấu nhất là dưới 10.000, trong vòng 2 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    from contextlib import redirect_stdout
    import io
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# Provided samples
assert run("5 5\n1 2\n2 2 3\n2 3 4\n2 4 5\n1 5\n") == "0", "sample 1"
assert run("3 2\n0\n0\n0\n") == "3", "all isolated"

# Custom cases
assert run("4 3\n1 1\n0\n2 1 2\n0\n") == "2", "two isolated employees"
assert run("2 2\n1 1\n1 2\n") == "1", "each employee knows a different language"
assert run("3 3\n1 1\n1 2\n1 3\n") == "2", "no overlap at all"
assert run("3 3\n2 1 2\n2 2 3\n1 3\n") == "0", "connected through chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 3\n1 1\n0\n2 1 2\n0\n`| 2 | Kết nối chính xác những nhân viên bị cô lập với các nhóm hiện có | 
|`2 2\n1 1\n1 2\n`| 1 | Hai nhân viên có ngôn ngữ khác nhau cần một bài học | 
|`3 3\n1 1\n1 2\n1 3\n`| 2 | Nhân viên hoàn toàn rời rạc | 
|`3 3\n2 1 2\n2 2 3\n1 3\n`| 0 | Nhân viên được kết nối thông qua các ngôn ngữ chồng chéo |
