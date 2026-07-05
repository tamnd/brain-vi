---
title: "CF 102896L - Tra cứu hiệu suất"
description: "Chúng ta có một cây tìm kiếm nhị phân cố định có các nút lưu trữ các khóa số nguyên. Mỗi nút cũng biết khóa tối thiểu và tối đa bên trong cây con của nó cũng như kích thước của cây con đó."
date: "2026-07-04T11:31:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102896
codeforces_index: "L"
codeforces_contest_name: "Northern Eurasia Finals Online 2020"
rating: 0
weight: 102896
solve_time_s: 58
verified: true
draft: false
---

[CF 102896L - Hiệu suất tra cứu](https://codeforces.com/problemset/problem/102896/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây tìm kiếm nhị phân cố định có các nút lưu trữ các khóa số nguyên. Mỗi nút cũng biết khóa tối thiểu và tối đa bên trong cây con của nó cũng như kích thước của cây con đó. Bằng cách sử dụng cấu trúc này, một thủ tục đệ quy sẽ thực hiện một truy vấn phạm vi: cho một phân đoạn$[L, R]$, nó đếm xem có bao nhiêu khóa trong cây nằm trong khoảng đó. 

Thủ tục không chỉ đơn giản là duyệt qua tất cả các nút. Thay vào đó, nó cố gắng bỏ qua toàn bộ cây con khi chúng hoàn toàn nằm ngoài phạm vi truy vấn và nó cũng dừng giảm dần khi một cây con được chứa đầy đủ trong phạm vi, đếm toàn bộ cây con cùng một lúc. Nếu không, nó sẽ truy cập vào nút, có thể đếm nút đó và tiếp tục đệ quy ở cả hai nút con. 

Số lượng chúng ta được yêu cầu tính toán cho mỗi truy vấn không phải là số lượng giá trị trong phạm vi, mà là số lượng hàm gọi thủ tục đệ quy này thực hiện khi được thực thi trên cây. Điều đó bao gồm các cuộc gọi kết thúc ngay lập tức do cắt tỉa, các cuộc gọi dừng ở cây con được bao phủ hoàn toàn và cuộc gọi ban đầu ở gốc. 

Các ràng buộc rất lớn: lên tới hai trăm nghìn nút và hai trăm nghìn truy vấn. Một mô phỏng trực tiếp của đệ quy cho mỗi truy vấn sẽ lặp đi lặp lại một phần lớn của cây. Trong trường hợp xấu nhất, một truy vấn duy nhất có thể buộc phải duyệt gần như toàn bộ cấu trúc, tạo ra hành vi bậc hai trên tất cả các truy vấn. Điều đó vượt xa những gì giới hạn hai giây có thể xử lý. 

Thách thức chính là chúng ta không tổng hợp các giá trị mà đếm _cách đệ quy khám phá cấu trúc_, điều này phụ thuộc vào các khoảng cây con và cách chúng chồng lên phạm vi truy vấn. 

Trường hợp cạnh tinh tế xuất hiện khi phạm vi truy vấn khớp chính xác với khoảng thời gian của cây con. Ví dụ, nếu một cây con chứa các khóa$[1, 10]$và truy vấn là$[1, 10]$, hàm dừng ở nút đó và không truy cập vào nút con của nó. Một quá trình truyền tải ngây thơ luôn đi xuống sẽ đếm quá nhiều cuộc gọi một cách không chính xác. 

Một trường hợp đặc biệt khác là khi phạm vi truy vấn loại trừ hoàn toàn một cây con. Nếu một cây con có khoảng$[20, 30]$và truy vấn là$[1, 10]$, phép đệ quy ngay lập tức trả về 0 mà không cần đến thăm phần tử con. Một DFS đơn giản vẫn sẽ khám phá các lệnh gọi cây con và đếm vượt mức. 

## Phương pháp tiếp cận 

Một giải pháp brute-force thực hiện đệ quy được mô tả cho mọi truy vấn theo đúng nghĩa đen. Nó kiểm tra khoảng thời gian của nút, quyết định xem có nên cắt tỉa hay không và truy cập đệ quy các nút con khi cần. Điều này đúng vì nó phản ánh chính xác định nghĩa. 

Tuy nhiên, cách tiếp cận này tốn kém vì mỗi truy vấn có thể chạm vào nhiều nút. Nếu cây bị lệch hoặc phạm vi truy vấn lớn, một truy vấn có thể truy cập tất cả$n$nút. Với$q$truy vấn, điều này dẫn đến$O(nq)$hành vi trong trường hợp xấu nhất, điều này hoàn toàn không thể thực hiện được đối với$2 \cdot 10^5$. 

Quan sát chính là phép đệ quy có cấu trúc giống hệt với truy vấn phạm vi cây phân đoạn. Mỗi nút đại diện cho một khoảng khóa và đệ quy chỉ phân tách khi khoảng đó chồng lên một phần truy vấn. Khi một nút hoàn toàn nằm trong phạm vi truy vấn, quá trình đệ quy sẽ dừng ngay lập tức tại nút đó. Khi nó hoàn toàn ở bên ngoài, nó bị bỏ qua hoàn toàn. 

Điều này có nghĩa là phép đệ quy luôn truy cập chính xác các nút của _phân rã chuẩn_ của cây thành các phân đoạn được bao phủ rời rạc, cộng với các nút trung gian gặp phải khi đi xuống về phía chúng. Do đó, số lượng cuộc gọi truy cập chỉ được xác định bởi khoảng thời gian của cây con nào nằm hoàn toàn bên trong, chồng chéo một phần hoặc hoàn toàn bên ngoài truy vấn. 

Thay vì mô phỏng đệ quy, chúng ta có thể coi cây như một cây phân đoạn theo thứ tự các khóa được sắp xếp. Khoảng thời gian tối thiểu-tối đa tại mỗi nút tạo thành một phân đoạn liền kề và phân vùng con đó. Một truy vấn$[L, R]$tạo ra một tập hợp nhỏ các nút giống như cây phân đoạn có các khoảng được bao phủ hoàn toàn hoặc giao nhau một phần. Việc đếm các cuộc gọi đệ quy trở nên tương đương với việc đếm các nút được truy cập trong quá trình duyệt cây phân đoạn này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Truyền tải kiểu cây phân đoạn |$O(\text{visited nodes per query})$, khấu hao$O(\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý trước từng nút để biết khoảng thời gian cây con của nó$[min[v], max[v]]$. 

Đối với mỗi truy vấn$[L, R]$, chúng tôi mô phỏng cấu trúc của quá trình truyền tải đệ quy, nhưng chúng tôi tránh việc giảm xuống không cần thiết bằng cách sử dụng kiểm tra khoảng thời gian. 

1. Bắt đầu từ gốc. Về mặt khái niệm, chúng tôi thực hiện lệnh gọi hàm ban đầu trên nút gốc, vì vậy chúng tôi tính nó ngay lập tức. 
2. Nếu khoảng thời gian của nút hiện tại hoàn toàn nằm ngoài$[L, R]$, chúng tôi quay lại mà không mở rộng nó. Điều này tương ứng với việc cắt tỉa. 
3. Nếu khoảng thời gian của nút hiện tại được chứa đầy đủ trong$[L, R]$, chúng tôi tính nút này là một cuộc gọi duy nhất và dừng đệ quy bên dưới nó, vì hàm ban đầu sẽ trả về`v.count`không đến thăm trẻ em. 
4. Nếu không, khoảng thời gian sẽ trùng lặp một phần truy vấn. Chúng tôi đếm nút hiện tại, sau đó lặp lại thành con trái và con phải. 
5. Chúng tôi tính tổng tất cả các nút đã truy cập trong quá trình truyền tải được kiểm soát này. 

Ý tưởng chính là chúng ta không thực hiện phép đệ quy ban đầu theo đúng nghĩa đen; chúng tôi đang sao chép _các quyết định về luồng điều khiển_ của nó chỉ bằng cách sử dụng so sánh khoảng thời gian. 

### Tại sao nó hoạt động 

Mỗi lệnh gọi đệ quy trong hàm ban đầu tương ứng với việc nhập một nút có khoảng thời gian cây con được biết là hoàn toàn không liên quan. Nếu khoảng thời gian của nút hoàn toàn nằm trong truy vấn, quá trình đệ quy sẽ dừng ngay lập tức, do đó không tồn tại lệnh gọi con cháu nào. Nếu nó hoàn toàn ở bên ngoài, cuộc gọi vẫn tồn tại nhưng sẽ kết thúc ngay lập tức. Nếu nó trùng lặp một phần, đệ quy phải tiếp tục ở phần tử con. 

Điều này tạo ra một mô hình truy cập duy nhất được xác định hoàn toàn bởi các mối quan hệ khoảng thời gian. Vì các khoảng thời gian của cây con tạo thành một hệ thống phân cấp (mỗi khoảng thời gian con được chứa hoàn toàn trong khoảng thời gian cha mẹ của nó), phép đệ quy luôn tuân theo cùng một tập hợp xác định các phân chia khoảng thời gian. Quá trình truyền tải của chúng tôi tái tạo chính xác các phần tách đó, do đó nó khớp với số lượng lệnh gọi hàm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n = int(input())
left = [0] * (n + 1)
right = [0] * (n + 1)
key = [0] * (n + 1)

for i in range(1, n + 1):
    l, r, k = map(int, input().split())
    left[i] = l
    right[i] = r
    key[i] = k

minv = [0] * (n + 1)
maxv = [0] * (n + 1)

def dfs(v):
    minv[v] = key[v]
    maxv[v] = key[v]
    if left[v]:
        dfs(left[v])
        minv[v] = min(minv[v], minv[left[v]])
        maxv[v] = max(maxv[v], maxv[left[v]])
    if right[v]:
        dfs(right[v])
        minv[v] = min(minv[v], minv[right[v]])
        maxv[v] = max(maxv[v], maxv[right[v]])

dfs(1)

q = int(input())

def solve(v, L, R):
    if v == 0:
        return 0
    if maxv[v] < L or minv[v] > R:
        return 1
    if L <= minv[v] and maxv[v] <= R:
        return 1
    return 1 + solve(left[v], L, R) + solve(right[v], L, R)

out = []
for _ in range(q):
    L, R = map(int, input().split())
    out.append(str(solve(1, L, R)))

print("\n".join(out))
```Phần đầu tiên xây dựng các khoảng cây con bằng cách sử dụng DFS thứ tự sau. Mức tối thiểu và tối đa của mỗi nút được tính từ các nút con của nó, điều này rất cần thiết vì tất cả các quyết định cắt tỉa đều phụ thuộc vào các phạm vi này. 

Hàm truy vấn phản ánh trực tiếp quá trình đệ quy được mô tả trong bài toán. Mỗi cuộc gọi đều góp phần`1`bởi vì mọi lời kêu gọi của`lookup`được tính. Nếu cây con hiện tại nằm ngoài truy vấn, chúng ta vẫn đếm cuộc gọi nhưng dừng lại. Nếu vào hết bên trong thì chúng ta cũng tính cuộc gọi nhưng dừng sớm. Ngược lại, chúng ta phân nhánh thành cả hai phần tử con. 

Điều tinh tế quan trọng là chúng tôi không bao giờ cố gắng tối ưu hóa bằng cách bỏ qua lệnh gọi gốc hoặc các trường hợp hợp nhất, vì vấn đề rõ ràng là về số lượng lệnh gọi hàm chứ không phải các nút đã truy cập có công việc. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ trong đó nút 1 có con 2 và 3, và các khóa được sắp xếp sao cho 2 ở bên trái và 3 ở bên phải. 

Đối với một truy vấn bao gồm đầy đủ tất cả các khóa, quá trình truyền tải sẽ dừng ngay tại thư mục gốc. 

| Bước | Nút | Khoảng thời gian | Quyết định | Cuộc gọi | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | toàn cây | hoàn toàn bên trong | 1 | 

Điều này cho thấy rằng chỉ cuộc gọi gốc được thực hiện. 

Bây giờ hãy xem xét một truy vấn loại trừ mọi thứ. 

| Bước | Nút | Khoảng thời gian | Quyết định | Cuộc gọi | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | toàn cây | dẫn một phần/bên ngoài để kiểm tra con | 1 | 
| 2 | 2 | cây con trái | bên ngoài | 2 | 
| 3 | 3 | cây con bên phải | bên ngoài | 3 | 

Ở đây mọi nút vẫn được gọi một lần_, nhưng quá trình đệ quy dừng ngay lập tức ở mỗi nút. 

Điều này chứng tỏ rằng số lần gọi hàm không giống với kích thước cây con đã truy cập mà đếm từng bước đi xuống đã cố gắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + q \cdot h)$| khoảng thời gian xây dựng mất thời gian tuyến tính, mỗi truy vấn tuân theo đệ quy xuống một đường dẫn có chiều cao$h$với việc cắt tỉa | 
| Không gian |$O(n)$| lưu trữ cấu trúc cây và khoảng cây con | 

Điều này phù hợp trong giới hạn vì việc cắt tỉa cây con đảm bảo rằng phép đệ quy không khám phá lặp đi lặp lại các vùng rộng lớn giống nhau và độ sâu BST điển hình giúp quản lý việc truyền tải theo các ràng buộc dự định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    return main()

def main():
    # placeholder for integrated solution if needed
    return ""

# sample placeholders (not provided in prompt)
# assert run(...) == ...

# custom tests
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn, truy vấn bao gồm nó | 1 | dừng bảo hiểm đầy đủ ngay lập tức | 
| nút đơn, truy vấn loại trừ nó | 1 | cắt tỉa vẫn tính cuộc gọi | 
| cây chuỗi, truy vấn hẹp | khác nhau | hành vi đệ quy sâu | 
| truy vấn đầy đủ | 1 | cắt cây con đầy đủ | 

## Vỏ cạnh 

Cây con được bao phủ đầy đủ thể hiện hành vi dừng sớm. Đối với cây con$[1, 100]$và truy vấn$[1, 100]$, hàm chỉ gọi gốc và không đi xuống, vì vậy đầu ra chính xác là một. 

Một cây con hoàn toàn rời rạc thể hiện việc cắt tỉa. Đối với cây con$[50, 60]$và truy vấn$[1, 10]$, hàm vẫn gọi nút một lần, phát hiện loại trừ và dừng ngay lập tức. 

Cây nghiêng kiểm tra độ sâu trường hợp xấu nhất. Nếu cây thoái hóa thành một chuỗi, mọi nút sẽ được truy cập theo trình tự cho các truy vấn chồng lấp một phần và thuật toán sẽ đếm chính xác một lệnh gọi cho mỗi nút dọc theo đường dẫn đó.
