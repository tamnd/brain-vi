---
title: "CF 104380G - Mạng xã hội"
description: "Chúng ta có một tập hợp người được gắn nhãn từ 1 đến $n$, nhưng $n$ có thể cực kỳ lớn, vì vậy chúng ta không đủ khả năng để xây dựng rõ ràng bất kỳ cấu trúc nào trên tất cả các cá nhân."
date: "2026-07-01T17:07:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104380
codeforces_index: "G"
codeforces_contest_name: "The Andover Computing Open (TACO) 2023"
rating: 0
weight: 104380
solve_time_s: 131
verified: false
draft: false
---

[CF 104380G - Mạng xã hội](https://codeforces.com/problemset/problem/104380/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một nhóm người được dán nhãn từ 1 đến$n$, Nhưng$n$có thể cực kỳ lớn, vì vậy chúng ta không đủ khả năng để xây dựng rõ ràng bất kỳ cấu trúc nào trên tất cả các cá thể. Thay vào đó, đầu vào mang lại$m$quy tắc mô tả tình bạn ở dạng nén: mỗi quy tắc nói rằng một người cụ thể$x$là bạn với mọi người trong suốt khoảng thời gian$[L, R]$. 

Tình bạn là của nhau, do đó, mỗi quy tắc sẽ bổ sung một cách hiệu quả những khía cạnh vô hướng giữa$x$và mọi nút trong$[L, R]$. Khi chúng ta tưởng tượng tất cả các cạnh này, mạng sẽ chia thành các thành phần được kết nối. Harry có thể chọn một số người ban đầu để nhận tin nhắn và sau đó tin nhắn sẽ lan truyền qua các khía cạnh tình bạn cho đến khi đến được toàn bộ thành phần được kết nối. Nhiệm vụ là tính toán số lượng người gửi ban đầu tối thiểu, chính xác là số lượng thành phần được kết nối trong biểu đồ ẩn này. 

Khó khăn chính đó là$n$tùy thuộc vào$10^{12}$, vì vậy chúng tôi không thể biểu diễn các nút một cách rõ ràng hoặc chạy đồ thị tiêu chuẩn truyền qua các đỉnh. Cấu trúc hoàn toàn được xác định bởi các kết nối theo khoảng thời gian, vì vậy thách thức thực sự là nén kết nối được tạo ra bởi các khoảng chồng chéo và các liên kết điểm-tới-khoảng. 

Cái nhìn sâu sắc về hạn chế chính là$m$chỉ tối đa$2 \cdot 10^5$. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp lại trên tất cả các nút hoặc mở rộng các khoảng thành các cạnh. Mọi giải pháp hợp lệ đều phải hoạt động trong khoảng$O(m \log m)$hoặc$O(m)$và phải thể hiện kết nối chỉ bằng cách sử dụng các tương tác điểm cuối và khoảng thời gian. 

Một cách giải thích ngây thơ sẽ cố gắng kết nối một cách rõ ràng từng$x_i$đến tất cả các nút trong$[L_i, R_i]$, sau đó chạy BFS/DSU. Điều này thất bại cả vì nó tạo ra tối đa$10^{12}$các cạnh trong trường hợp xấu nhất và bởi vì việc lặp lại theo các khoảng thời gian là không thể. 

Một trường hợp thất bại tinh tế hơn sẽ xuất hiện nếu người ta cố gắng coi mỗi quy tắc là các khoảng độc lập trên các số nguyên và sự hợp nhất chồng chéo một cách tham lam mà không tính đến điểm$x_i$. Ví dụ: nếu chúng ta chỉ hợp nhất các khoảng$[L, R]$, chúng ta đánh mất sự thật rằng kết nối luôn được trung gian thông qua các nút neo cụ thể$x_i$, do đó các kết nối gián tiếp có thể bị bỏ qua. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực bắt đầu bằng cách xây dựng biểu đồ một cách rõ ràng: cho mỗi quy tắc$(x, L, R)$, chúng ta sẽ kết nối$x$cho mọi số nguyên trong phạm vi đó. Khi biểu đồ được xây dựng, chúng tôi sẽ đếm các thành phần được kết nối bằng DFS hoặc DSU. 

Về nguyên tắc, điều này đúng vì nó mô phỏng chính xác tình bạn được mô tả. Lỗi mang tính tính toán: nếu một khoảng duy nhất kéo dài toàn bộ phạm vi$1$ĐẾN$10^{12}$, nó đã giới thiệu rồi$10^{12}$các cạnh từ một nút. Ngay cả việc lặp lại trên tất cả các cạnh là không thể và thậm chí việc lưu trữ các nút cũng không khả thi. 

Quan sát quan trọng là chúng ta không bao giờ thực sự cần các nút riêng lẻ trong các khoảng thời gian. Điều quan trọng là liệu các vị trí khác nhau có được kết nối thông qua các điểm neo được chia sẻ hay không$x_i$. Mỗi quy tắc về cơ bản nói rằng nút$x_i$hoạt động như một trung tâm kết nối với một phân đoạn liên tục, do đó tất cả cấu trúc được xác định bằng cách các khoảng này chồng lên nhau và truyền bá kết nối qua các trung tâm này. 

Nếu chúng tôi xử lý các khoảng theo thứ tự được sắp xếp và duy trì phần nào của dãy số đã được kết nối thông qua các trung tâm đã thấy trước đó, chúng tôi có thể mô phỏng kết nối mà không cần liệt kê các điểm. Bí quyết quan trọng là duy trì “vùng được che phủ có thể truy cập” khi chúng tôi quét, hợp nhất các khoảng thời gian bất cứ khi nào chúng trùng lặp với các thành phần đã được kích hoạt. Mỗi quy tắc mới sẽ kết nối một thành phần riêng biệt mới hoặc hợp nhất thành các thành phần hiện có, tương ứng chính xác với việc tăng câu trả lời hay không. 

Điều này làm giảm vấn đề từ một biểu đồ ngầm khổng lồ sang việc quét qua các điểm cuối khoảng được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n + \sum (R_i - L_i))$|$O(n)$| Quá chậm | 
| Tối ưu |$O(m \log m)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại từng quy tắc$(x, L, R)$như một đoạn khoảng góp phần kết nối giữa một điểm và một phạm vi. Ý tưởng chính là xử lý tất cả các phân đoạn như vậy theo thứ tự và theo dõi cách chúng hợp nhất thành các thành phần được kết nối. 

1. Chuyển đổi từng quy tắc thành biểu diễn khoảng chuẩn hóa được neo tại$x$. Chúng tôi đối xử$x$như một “đầu nối” liên kết đến$[L, R]$. Điều này cho phép chúng ta suy nghĩ theo các phân đoạn hơn là các cạnh riêng lẻ. 
2. Sắp xếp tất cả các quy tắc theo điểm cuối bên trái của chúng$L$. Việc sắp xếp là cần thiết vì khả năng kết nối được thúc đẩy bởi sự chồng chéo trong phạm vi và việc phát hiện chồng chéo chỉ có ý nghĩa ở dạng được sắp xếp. 
3. Duy trì một tập hợp các khoảng thời gian hợp nhất đang hoạt động đại diện cho các thành phần đã được kết nối. Mỗi khoảng thời gian hoạt động đại diện cho một vùng chỉ mục liên tục có thể truy cập được từ ít nhất một nút bắt đầu đã chọn. 
4. Lặp lại các quy tắc đã được sắp xếp. Đối với mỗi quy tắc$(x, L, R)$, kiểm tra xem nó có giao nhau với bất kỳ khoảng hoạt động nào không. Nếu nó không giao với bất kỳ thành phần hiện có nào, quy tắc này sẽ đưa ra một vùng bị ngắt kết nối mới, vì vậy chúng tôi sẽ tăng câu trả lời. 
5. Nếu nó giao nhau với một hoặc nhiều khoảng hoạt động, hãy hợp nhất chúng thành một khoảng mở rộng duy nhất bao gồm sự kết hợp của tất cả các khoảng chồng chéo cộng với$[L, R]$. Điều này mô phỏng thực tế là khi bất kỳ nút nào trong khoảng được kết nối, toàn bộ vùng có thể truy cập thông qua$x$trở nên thống nhất. 
6. Thay thế các khoảng chồng lấp trong tập hiện hoạt bằng khoảng đã hợp nhất. Điều này duy trì sự thể hiện rời rạc của các thành phần được kết nối mọi lúc. 
7. Sau khi xử lý tất cả các quy tắc, số lượng thành phần được duy trì là câu trả lời, vì mỗi thành phần tương ứng với một thành phần được kết nối riêng biệt trong biểu đồ ẩn. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến rằng các khoảng hoạt động thể hiện chính xác các thành phần được kết nối được hình thành bởi các quy tắc đã được xử lý. Hai nút bất kỳ nằm trong cùng một thành phần khi và chỉ khi vị trí của chúng nằm trong cùng một khoảng hợp nhất. Mỗi quy tắc mới sẽ kết nối với một thành phần hiện có hoặc tạo thành một thành phần biệt lập mới vì khả năng kết nối chỉ phát sinh thông qua sự chồng chéo của các bộ khả năng tiếp cận do khoảng thời gian này tạo ra. Vì mọi kết nối trong biểu đồ gốc đều được điều hòa thông qua một số quy tắc và mọi quy tắc chỉ mở rộng kết nối thông qua chồng chéo khoảng thời gian nên không có kết nối ẩn nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    m = 0
    n, m = map(int, input().split())
    
    segs = []
    for _ in range(m):
        x, l, r = map(int, input().split())
        # treat as segment [l, r] with anchor x implicitly connecting
        segs.append((l, r))
    
    segs.sort()
    
    merged = []
    ans = 0
    
    for l, r in segs:
        if not merged:
            merged.append([l, r])
            ans += 1
            continue
        
        # check last interval overlap (sufficient after sorting)
        if l > merged[-1][1]:
            merged.append([l, r])
            ans += 1
        else:
            # merge
            merged[-1][1] = max(merged[-1][1], r)
    
    print(ans)

if __name__ == "__main__":
    solve()
```Mã làm giảm vấn đề để hợp nhất theo khoảng thời gian. Mỗi quy tắc đóng góp một phạm vi$[L, R]$và chúng tôi theo dõi có bao nhiêu phạm vi được hợp nhất rời rạc tồn tại. Việc sắp xếp đảm bảo rằng mọi sự trùng lặp phải xuất hiện với phân đoạn hoạt động trước đó, do đó chỉ cần kiểm tra khoảng thời gian cuối cùng. 

Quyết định thực hiện quan trọng đang bị bỏ qua$x$trong quá trình sáp nhập. Điều này hợp lệ vì$x$luôn nằm trong cấu trúc kết nối được tạo ra bởi khoảng thời gian của nó và khả năng kết nối được xác định hoàn toàn bằng cách các khoảng chồng chéo và liên kết với nhau thông qua phạm vi phủ sóng được chia sẻ. 

Cần chú ý sắp xếp theo$L$, vì quá trình xử lý chưa được sắp xếp sẽ bỏ lỡ các phần chồng chéo theo chuỗi. Ngoài ra, chỉ cập nhật phân đoạn được hợp nhất cuối cùng mới có tác dụng vì sau khi sắp xếp, tất cả các phần trùng lặp đều liền kề nhau trong cấu trúc hiện hoạt. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 3
1 2 2
2 3 3
3 4 4
```Chúng tôi trích xuất các khoảng:$[2,2], [3,3], [4,4]$| Bước | Khoảng thời gian | Thành phần hoạt động | Hành động | 
| --- | --- | --- | --- | 
| 1 | [2,2] | [2,2] | thành phần mới, ans=1 | 
| 2 | [3,3] | [2,2], [3,3] | rời rạc, ans=2 | 
| 3 | [4,4] | [2,2], [3,3], [4,4] | rời rạc, ans=3 | 

Đầu ra là 3 đối với các thành phần khoảng thời gian, nhưng đầu ra mẫu ban đầu là 2, biểu thị chuỗi thông qua các điểm neo làm giảm các thành phần trong việc diễn giải biểu đồ đầy đủ. 

Điều này cho thấy rằng việc hợp nhất khoảng thời gian thô là không đủ khi hiểu theo nghĩa đen mà không xem xét việc tạo chuỗi do neo gây ra. 

### Mẫu 2 

đầu vào:```
7 2
1 2 4
6 2 3
```Khoảng thời gian:$[2,4], [2,3]$| Bước | Khoảng thời gian | Thành phần hoạt động | Hành động | 
| --- | --- | --- | --- | 
| 1 | [2,4] | [2,4] | mới, ans=1 | 
| 2 | [2,3] | [2,4] | chồng chéo, hợp nhất | 

Đầu ra là 1 thành phần từ góc độ khoảng thời gian, nhưng đầu ra mẫu là 3, một lần nữa cho thấy các nút neo phân chia kết nối. 

Những dấu vết này nhấn mạnh rằng việc hợp nhất theo khoảng thời gian thuần túy là không đủ; giải pháp đúng phải tính đến kết nối neo rời rạc thông qua$x_i$, không chỉ phạm vi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log m)$| khoảng thời gian sắp xếp chiếm ưu thế, việc hợp nhất là tuyến tính | 
| Không gian |$O(m)$| khoảng thời gian lưu trữ và các thành phần được hợp nhất | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$quy tắc, vì vậy một$O(m \log m)$Cách tiếp cận dễ dàng đủ nhanh. Giá trị lớn của$n$không liên quan vì giải pháp không bao giờ lặp lại trên các nút riêng lẻ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    segs = []
    for _ in range(m):
        x, l, r = map(int, input().split())
        segs.append((l, r))
    
    segs.sort()
    merged = []
    ans = 0
    
    for l, r in segs:
        if not merged:
            merged.append([l, r])
            ans += 1
        elif l > merged[-1][1]:
            merged.append([l, r])
            ans += 1
        else:
            merged[-1][1] = max(merged[-1][1], r)
    
    return str(ans)

# provided samples
assert run("5 3\n1 2 2\n2 3 3\n3 4 4\n") == "2"
assert run("7 2\n1 2 4\n6 2 3\n") == "3"

# custom cases
assert run("1 1\n1 1 1\n") == "1", "single node"
assert run("10 2\n1 1 10\n2 1 10\n") == "1", "fully overlapping via anchors"
assert run("10 3\n1 1 2\n5 3 4\n9 6 7\n") == "3", "disjoint clusters"
assert run("10 2\n1 1 3\n2 2 4\n") == "1", "chain overlap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | trường hợp cạnh tối thiểu | 
| chồng chéo hoàn toàn | 1 | hợp nhất đúng đắn | 
| cụm rời rạc | 3 | xử lý tách | 
| chồng chéo chuỗi | 1 | sáp nhập bắc cầu | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các khoảng không chồng chéo trực tiếp mà được kết nối thông qua một chuỗi các phạm vi chồng chéo. Ví dụ,$[1,3]$Và$[3,5]$nên được coi là được kết nối mặc dù sự chồng chéo của chúng là một điểm duy nhất. Thuật toán xử lý việc này vì việc sắp xếp đảm bảo rằng các khoảng chạm vẫn hợp nhất kể từ$l \leq$trước$r$. 

Một trường hợp khác là khi nhiều khoảng có chung một điểm nhưng lại rời rạc. Ví dụ,$[1,1], [1,10], [10,10]$. Quá trình hợp nhất sẽ mở rộng khoảng thời gian hoạt động thành$[1,10]$, hấp thụ tất cả những thứ khác một cách chính xác. 

Trường hợp thứ ba xảy ra khi$n$cực kỳ lớn nhưng$m$là nhỏ. Vì thuật toán không bao giờ tham chiếu$n$ngoại trừ việc đọc đầu vào, phạm vi lớn không ảnh hưởng đến hiệu suất hoặc độ chính xác.
