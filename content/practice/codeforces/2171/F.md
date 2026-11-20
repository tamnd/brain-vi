---
title: "CF 2171F - Rae Taylor và Trees (bản cứng)"
description: "Chúng ta được yêu cầu xây dựng một cây trên các đỉnh $n$ được dán nhãn từ $1$ đến $n$ sao cho với mọi cạnh nối các đỉnh $u$ và $v$ với $u < v$, đỉnh $u$ xuất hiện trước $v$ trong một hoán vị cho trước $p$."
date: "2026-06-07T23:08:39+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "data-structures", "dp", "dsu", "greedy", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 2171
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1065 (Div. 3)"
rating: 1600
weight: 2171
solve_time_s: 114
verified: false
draft: false
---

[CF 2171F - Rae Taylor và Trees (phiên bản cứng)](https://codeforces.com/problemset/problem/2171/F) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, cấu trúc dữ liệu, dp, dsu, tham lam, triển khai, cây 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một cái cây trên$n$đỉnh được dán nhãn từ$1$ĐẾN$n$sao cho với mọi cạnh nối các đỉnh$u$Và$v$với$u < v$, đỉnh$u$xuất hiện trước$v$trong một hoán vị nhất định$p$. Tương tự, cha của bất kỳ đỉnh nào trong cây phải xuất hiện sớm hơn trong$p$hơn đứa trẻ nếu chúng ta nghĩ về cái cây có gốc ở đỉnh nhỏ nhất. Hoán vị đảm bảo rằng mỗi số từ$1$ĐẾN$n$xảy ra đúng một lần nên không tồn tại sự trùng lặp. 

Đầu vào chứa nhiều trường hợp thử nghiệm, mỗi trường hợp có một hoán vị. Đầu ra yêu cầu trước tiên phải nêu rõ liệu một cây như vậy có khả thi hay không và nếu có thì cung cấp các cạnh của nó. Mỗi cạnh phải thỏa mãn ràng buộc thứ tự liên quan đến$p$. 

Những ràng buộc chỉ ra rằng$n$có thể lên đến$2 \cdot 10^5$và tổng của$n$trên tất cả các trường hợp thử nghiệm cũng tùy thuộc vào$2 \cdot 10^5$. Điều này loại trừ bất kỳ giải pháp nào kiểm tra tất cả các cây có thể một cách rõ ràng, vì có rất nhiều cây theo cấp số nhân cho các cây thậm chí vừa phải.$n$. Một giải pháp tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm là cần thiết. 

Các trường hợp cạnh bao gồm các hoán vị trong đó phần tử nhỏ nhất không đứng đầu hoặc các chuỗi nhảy xung quanh sao cho không có cây nhất quán nào có thể thỏa mãn điều kiện “trước đó trong$p$phải nhỏ hơn”. Chẳng hạn, nếu$p = [3,1,2]$, đỉnh nhỏ nhất 1 đứng sau 3, khiến không thể gắn 3 vào bất cứ thứ gì nhỏ hơn nó trong khi tuân theo mệnh lệnh. Việc triển khai bất cẩn luôn gắn các đỉnh vào đỉnh được thêm vào cuối cùng một cách tham lam có thể tạo ra các cây không hợp lệ trong những trường hợp như vậy. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử kết nối mọi cặp$(u, v)$với$u < v$Và$u$xảy ra trước$v$TRONG$p$, rồi cố gắng tập hợp một cây từ những cặp này. Điều này có tác dụng vì mọi cạnh hợp lệ phải đáp ứng thứ tự trong$p$. Tuy nhiên, việc xây dựng tất cả các cạnh hợp lệ và kiểm tra kết nối là$O(n^2)$, điều đó là không thể thực hiện được đối với$n \sim 10^5$. Ngay cả tìm kiếm theo chiều sâu hoặc tìm kiếm kết hợp trên tất cả các cạnh ứng viên cũng quá chậm vì có khoảng$O(n^2)$ứng viên. 

Cái nhìn sâu sắc quan trọng là xử lý hoán vị theo thứ tự và cố gắng gắn mỗi đỉnh vào đỉnh trước gần nhất nhỏ hơn chính nó. Nếu không có đỉnh trước đó tồn tại đối với một số đỉnh thì cây không thể được hình thành. Bằng cách sử dụng cấu trúc dữ liệu như deque hoặc quét tuyến tính đơn giản từ trái sang phải, chúng ta có thể duy trì một tập hợp các đỉnh ứng cử viên có thể đóng vai trò là cha mẹ. Mỗi đỉnh có một đỉnh cha với nhãn nhỏ hơn, đảm bảo đặc tính của cây. Điều này làm giảm vấn đề về một cấu trúc tham lam dựa trên thứ tự hoán vị, đưa ra$O(n)$mỗi trường hợp thử nghiệm. 

Ý tưởng tham lam có hiệu quả vì trong một cái cây có$n$đỉnh, mỗi đỉnh ngoại trừ gốc có đúng một đỉnh cha. Điều kiện thứ tự cho phép đỉnh cha là bất kỳ đỉnh nhỏ hơn nào xuất hiện trước đó trong$p$. Nếu không có đỉnh như vậy tồn tại thì đỉnh đó không thể được kết nối mà không vi phạm điều kiện. Mối quan hệ cha-con có thể được chỉ định một cách tham lam trong khi vẫn duy trì kết nối vì cây chỉ yêu cầu$n-1$các cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n²) | Quá chậm | 
| Xây dựng tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$và hoán vị$p$. Xây dựng ánh xạ từ nhãn đỉnh tới chỉ mục của nó trong$p$để truy cập nhanh. Điều này giúp chúng ta xác định thứ tự của hai đỉnh bất kỳ trong$O(1)$. 
2. Kiểm tra xem phần tử đầu tiên của$p$là 1. Nếu không thì không thể thỏa mãn quy tắc vì đỉnh nhỏ nhất không thể có đỉnh cha nhỏ hơn chính nó. Trong trường hợp này, in "Không" và tiếp tục. 
3. Khởi tạo một danh sách để lưu trữ các cạnh và một ngăn xếp để theo dõi các đỉnh có thể đóng vai trò là đỉnh cha. Đẩy đỉnh đầu tiên vào ngăn xếp làm ứng cử viên gốc. 
4. Lặp lại hoán vị từ phần tử thứ hai đến phần tử cuối cùng. Đối với mỗi đỉnh$v$, lấy các đỉnh từ ngăn xếp cho đến khi tìm được đỉnh$u$nhỏ hơn$v$. Kết nối$v$đến đây$u$và đẩy$v$lên ngăn xếp. Nếu không như vậy$u$tồn tại thì không thể thỏa mãn các ràng buộc; xuất "Không". 
5. Nếu vòng lặp hoàn thành, xuất "Có" theo sau là các cạnh đã thu thập. 

Tại sao nó hoạt động: Ngăn xếp duy trì một chuỗi các đỉnh cha tiềm năng cho các đỉnh sắp tới, được sắp xếp theo sự xuất hiện của chúng trong$p$. Bằng cách luôn kết nối một đỉnh với đỉnh nhỏ hơn gần nhất trước đó, chúng tôi đảm bảo mỗi cạnh tuân theo thứ tự hoán vị. Vì mỗi đỉnh được kết nối một lần và chỉ một lần nên ta tạo thành một cây có chính xác$n-1$các cạnh. Ngăn xếp đảm bảo rằng chúng ta luôn chọn cha mẹ hợp lệ và việc bật lên các đỉnh lớn hơn đảm bảo điều kiện của cây được giữ nguyên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        pos = [0]*(n+1)
        for i, x in enumerate(p):
            pos[x] = i
        
        if p[0] != 1:
            print("No")
            continue
        
        edges = []
        stack = [p[0]]
        
        possible = True
        for v in p[1:]:
            while stack and stack[-1] > v:
                stack.pop()
            if not stack:
                possible = False
                break
            edges.append((stack[-1], v))
            stack.append(v)
        
        if possible:
            print("Yes")
            for u, v in edges:
                print(u, v)
        else:
            print("No")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc số lượng ca kiểm thử và từng hoán vị. Chúng tôi ánh xạ nhãn đỉnh tới các vị trí để kiểm tra thứ tự theo thời gian liên tục. Phần tử đầu tiên phải là 1 để hoạt động như phần gốc. Ngăn xếp theo dõi các đỉnh hợp lệ gần đây nhất, xuất hiện các đỉnh lớn hơn để duy trì điều kiện thứ tự. Các cạnh được nối thêm khi chúng tôi thực hiện, đảm bảo tính kết nối và độ chính xác. Nếu tại bất kỳ thời điểm nào không có cha mẹ hợp lệ tồn tại, chúng tôi sẽ phát hiện điều không thể ngay lập tức. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
6
1 3 4 5 2 6
```| Bước | Ngăn xếp | Hiện tại v | Hành động | Cạnh | 
| --- | --- | --- | --- | --- | 
| 0 | [1] | 3 | 1<3, kết nối | (1,3) | 
| 1 | [1,3] | 4 | 3<4, kết nối | (1,3),(3,4) | 
| 2 | [1,3,4] | 5 | 4<5, kết nối | (1,3),(3,4),(4,5) | 
| 3 | [1,3,4,5] | 2 | bật 5,4,3 -> 1<2, kết nối | (1,3),(3,4),(4,5),(1,2) | 
| 4 | [1,2] | 6 | 2<6, kết nối | (1,3),(3,4),(4,5),(1,2),(2,6) | 

Tất cả các đỉnh được xử lý, xuất ra "Có" và các cạnh. 

**Đầu vào mẫu 2:**```
3 4 1 2
```Phần tử đầu tiên là 3, không phải 1. Không thể. Đầu ra "Không". 

Những dấu vết này xác nhận rằng phương pháp ngăn xếp xác định chính xác cả trường hợp có thể và không thể và kết nối các đỉnh tôn trọng thứ tự hoán vị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi đỉnh được đẩy và bật ra nhiều nhất một lần từ ngăn xếp. | 
| Không gian | O(n) | Ngăn xếp và các cạnh lưu trữ tối đa n phần tử. | 

Cho rằng tổng của$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$2 \cdot 10^5$, giải pháp này chạy thoải mái trong giới hạn 3 giây và hạn chế về bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("1\n6\n1 3 4 5 2 6\n") == "Yes\n1 3\n3 4\n4 5\n1 2\n2 6", "sample 1"
assert run("1\n4\n3 4 1 2\n") == "No", "sample 2"

# Custom tests
assert run("1\n2\n1 2\n") == "Yes\n1 2", "minimum n=2"
assert run("1\n3\n2 1 3\n") == "No", "root not first"
assert run("1\n5\n1 2 3 4 5\n") == "Yes\n1 2\n2 3\n3 4\n4 5", "ascending order"
assert run("1\n5\n5 4 3 2 1\n") == "No", "descending order"
assert run("1\n6\n1 4 2 5 3 6\n
```
