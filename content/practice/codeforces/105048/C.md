---
title: "CF 105048C - Tranh chữ"
description: "Chúng tôi đang duy trì một mạng lưới các ngăn xếp. Mỗi ô trong bảng $N nhân N$ lưu trữ một chồng chữ cái theo chiều dọc, trong đó mỗi lần cập nhật sẽ đẩy một chữ cái lên ngăn xếp, bật chữ cái trên cùng hoặc hỏi xem ngăn xếp hiện tại có chữ cái “thống trị” hay không."
date: "2026-06-28T05:41:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105048
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 03-22-24 Div. 2 (Beginner)"
rating: 0
weight: 105048
solve_time_s: 104
verified: false
draft: false
---

[CF 105048C - Bức tranh dài dòng](https://codeforces.com/problemset/problem/105048/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một mạng lưới các ngăn xếp. Mỗi ô trong một$N \times N$bảng lưu trữ một chồng các chữ cái theo chiều dọc, trong đó mỗi bản cập nhật sẽ đẩy một chữ cái lên ngăn xếp, bật chữ cái trên cùng hoặc hỏi xem ngăn xếp hiện tại có chữ cái "thống trị" hay không. 

Một ngăn xếp được coi là đẹp trong một truy vấn nếu chữ cái được truy vấn xuất hiện đúng hơn một nửa số phần tử hiện có trong ngăn xếp đó. 

Vì vậy, mỗi thao tác được bản địa hóa thành một ô duy nhất, nhưng khó khăn chính là trình tự động: các thao tác đẩy và bật thay đổi chiều cao ngăn xếp theo thời gian và các truy vấn phải phản ánh nhiều tập hợp chữ cái hiện tại trong ngăn xếp đó. 

Các ràng buộc là tín hiệu thực sự. Lưới nhỏ ($N \le 100$), do đó có tối đa 10.000 ngăn xếp độc lập. Tuy nhiên, số lượng hoạt động có thể đạt tới$2 \cdot 10^5$. Điều này loại trừ mọi hoạt động quét ngăn xếp theo truy vấn vì một ngăn xếp có thể phát triển tuyến tính theo các thao tác. Trong trường hợp xấu nhất, việc đẩy liên tục đến một ô sẽ tạo ra một chồng kích thước$2 \cdot 10^5$và việc quét nó cho mỗi truy vấn sẽ ngay lập tức vượt quá giới hạn thời gian. 

Một cách tiếp cận đơn giản đếm tần số trong ngăn xếp cho mỗi truy vấn sẽ tốn kém$O(k)$mỗi truy vấn, ở đâu$k$là chiều cao ngăn xếp, dẫn đến$O(Q^2)$trong trường hợp xấu nhất. 

Một trường hợp khó phát hiện xuất phát từ việc xóa các ngăn xếp trống. Thao tác xóa trên một ô chưa được xử lý sẽ không có tác dụng gì, nhưng việc triển khai bất cẩn cho rằng ngăn xếp tồn tại có thể gây ra lỗi hoặc số lượng giảm không chính xác dưới 0. 

Một tình huống phức tạp khác là các thao tác xen kẽ trên cùng một ô: 

đầu vào:```
0 a 0 0
0 b 0 0
2 a 0 0
1 0 0
2 a 0 0
```Hành vi đúng yêu cầu theo dõi thứ tự chính xác: sau khi đẩy`a`, sau đó`b`, ngăn xếp là`[a, b]`, do đó không có chữ cái nào chiếm đa số; sau khi bật lên, ngăn xếp là`[a]`, Vì thế`a`trở nên thống trị. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản: đối với mỗi ô, hãy duy trì một danh sách đại diện cho ngăn xếp của nó. Một lệnh đẩy sẽ được thêm vào, một lệnh bật lên sẽ loại bỏ phần tử cuối cùng nếu có và một truy vấn đếm số lần xuất hiện của chữ cái đích trong toàn bộ ngăn xếp và kiểm tra xem nó có vượt quá một nửa kích thước ngăn xếp hay không. 

Điều này đúng vì nó mô phỏng trực tiếp định nghĩa. Vấn đề là hiệu suất. Mỗi truy vấn yêu cầu quét tới kích thước ngăn xếp và trong các trường hợp đối nghịch, kích thước ngăn xếp tăng lên$O(Q)$. Với tối đa$2 \cdot 10^5$hoạt động, điều này trở thành$O(Q^2)$, quá chậm. 

Quan sát quan trọng là chúng ta không cần thông tin tần số đầy đủ cho tất cả các chữ cái, chỉ dành cho thư ứng viên được truy vấn tại thời điểm đó. Tuy nhiên, ngay cả việc chỉ duy trì số lượng cũng không đủ nếu chúng ta muốn xác minh phần lớn một cách linh hoạt khi xóa, vì việc xóa các chữ cái tùy ý khỏi ngăn xếp sẽ làm thay đổi số dư theo thời gian. 

Cấu trúc trở nên đơn giản hơn nếu chúng ta duy trì, đối với mỗi ô, một bản đồ tần số của các chữ cái trong ngăn xếp hiện tại. Sau đó, mỗi lần đẩy sẽ tăng một số, mỗi lần bật sẽ giảm số lượng chữ cái trên cùng đã bị loại bỏ và chúng tôi cũng duy trì kích thước ngăn xếp hiện tại. Điều này làm cho mỗi thao tác$O(1)$trung bình và mỗi truy vấn cũng$O(1)$: chúng tôi chỉ kiểm tra xem`freq[l] > size/2`. 

Phần còn thiếu duy nhất là biết chữ cái nào cần giảm trong một cửa sổ bật lên, yêu cầu lưu trữ nội dung ngăn xếp thực tế chứ không chỉ đếm. 

Điều này dẫn đến thiết kế tối ưu: mỗi ô lưu trữ cả một ngăn xếp (cho thứ tự) và một từ điển tần số (để đếm). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (quét ngăn xếp cho mỗi truy vấn) |$O(Q^2)$|$O(Q)$| Quá chậm | 
| Ngăn xếp + bản đồ tần số trên mỗi ô |$O(Q)$|$O(N^2 + Q)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo lưới kích thước$N \times N$, trong đó mỗi ô chứa một ngăn xếp trống và một bản đồ tần số trống. 

Điều này cho phép chúng tôi tách biệt các hoạt động trên mỗi tọa độ để không cần tính toán lại toàn cục. 
2. Đối với hoạt động sơn tại$(x, y)$, đẩy chữ cái lên ngăn xếp tại ô đó và tăng bộ đếm tần số của nó. 

Ngăn xếp đảm bảo chúng ta có thể hoàn tác việc này một cách chính xác sau này và bộ đếm sẽ giữ thông tin tổng hợp theo thời gian không đổi. 
3. Đối với thao tác xóa tại$(x, y)$, kiểm tra xem ngăn xếp có trống không. Nếu nó trống, không làm gì cả. 

Nếu không trống, hãy bật phần tử trên cùng và giảm tần số của nó. Điều này đảm bảo tính nhất quán giữa thứ tự và số lượng. 
4. Đối với một truy vấn tại$(x, y)$với lá thư$l$, tính kích thước ngăn xếp hiện tại. Nếu bằng 0 thì xuất ra “no”. 

Ngược lại hãy kiểm tra xem`frequency[l] * 2 > size`. Điều này trực tiếp mã hóa đa số nghiêm ngặt mà không cần hoạt động dấu phẩy động. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, ngăn xếp của mỗi ô được thể hiện chính xác bằng danh sách được lưu trữ của nó và bản đồ tần số khớp chính xác với nhiều tập hợp các chữ cái trong ngăn xếp đó. Các thao tác đẩy và bật duy trì tính bất biến này vì mỗi lần chèn hoặc xóa sẽ cập nhật cả hai cấu trúc một cách nhất quán. Do đó, khi trả lời một truy vấn, bản đồ tần số sẽ cung cấp số lượng chính xác của chữ cái được truy vấn trong ngăn xếp hiện tại và so sánh nó với một nửa kích thước ngăn xếp sẽ xác định chính xác liệu nó có tạo thành đa số nghiêm ngặt hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, Q = map(int, input().split())
    
    grid = [[[] for _ in range(N)] for _ in range(N)]
    freq = [[dict() for _ in range(N)] for _ in range(N)]
    size = [[0 for _ in range(N)] for _ in range(N)]
    
    out = []
    
    for _ in range(Q):
        parts = input().split()
        s = int(parts[0])
        
        if s == 0:
            l = parts[1]
            x = int(parts[2])
            y = int(parts[3])
            
            grid[x][y].append(l)
            size[x][y] += 1
            freq[x][y][l] = freq[x][y].get(l, 0) + 1
        
        elif s == 1:
            x = int(parts[1])
            y = int(parts[2])
            
            if size[x][y] == 0:
                continue
            
            ch = grid[x][y].pop()
            size[x][y] -= 1
            freq[x][y][ch] -= 1
            if freq[x][y][ch] == 0:
                del freq[x][y][ch]
        
        else:
            l = parts[1]
            x = int(parts[2])
            y = int(parts[3])
            
            if size[x][y] == 0:
                out.append("no")
            else:
                if freq[x][y].get(l, 0) * 2 > size[x][y]:
                    out.append("yes")
                else:
                    out.append("no")
    
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp giữ một ngăn xếp trên mỗi ô để duy trì trật tự và hỗ trợ hành vi bật lên chính xác. Từ điển tần số được cập nhật theo từng bước với các thay đổi về ngăn xếp, đảm bảo kiểm tra phần lớn theo thời gian liên tục trong quá trình truy vấn. Sự riêng biệt`size`mảng tránh tính toán lại độ dài nhiều lần. 

Một chi tiết triển khai tinh tế là xóa các mục từ điển khi số lượng của chúng giảm xuống 0. Điều này không bắt buộc để đảm bảo tính chính xác nhưng nó giữ cho bộ nhớ được giới hạn và tránh các phím sai trong quá trình thực hiện.`.get()`séc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một ô duy nhất$(0,0)$: 

| Bước | Hoạt động | Ngăn xếp | Tần số | Kích thước | Kết quả truy vấn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | đẩy một | [a] | {a:1} | 1 | - | 
| 2 | đẩy b | [a,b] | {a:1,b:1} | 2 | - | 
| 3 | truy vấn | [a,b] | {a:1,b:1} | 2 | không | 
| 4 | bật | [a] | {a:1} | 1 | - | 
| 5 | truy vấn | [a] | {a:1} | 1 | vâng | 

Điều này cho thấy phần lớn phụ thuộc vào trạng thái ngăn xếp hiện tại chứ không phải sự thống trị trong lịch sử. 

### Ví dụ 2 

Ô đơn với các lần đẩy lặp đi lặp lại: 

| Bước | Hoạt động | Ngăn xếp | Tần số | Kích thước | Truy vấn l='a' | 
| --- | --- | --- | --- | --- | --- | 
| 1 | đẩy một | [a] | {a:1} | 1 | vâng | 
| 2 | đẩy b | [a,b] | {a:1,b:1} | 2 | không | 
| 3 | đẩy một | [a,b,a] | {a:2,b:1} | 3 | vâng | 

Điều này chứng tỏ rằng cấu trúc tần số theo dõi chính xác phần lớn đang phát triển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(Q)$| Mỗi thao tác cập nhật hoặc truy vấn cấu trúc thời gian không đổi trên mỗi ô | 
| Không gian |$O(N^2 + Q)$| Lưu trữ lưới cộng với tổng số phần tử được đẩy trên tất cả các ngăn xếp | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$hoạt động, vì vậy thời gian tuyến tính với các cập nhật bản đồ băm là đủ trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# minimal single cell behavior
assert run("""1 5
0 a 0 0
0 b 0 0
2 a 0 0
1 0 0
2 a 0 0
""") == "no\nyes"

# all same letter always majority
assert run("""2 4
0 a 0 0
0 a 0 0
2 a 0 0
2 b 0 0
""") == "yes\nno"

# empty stack query
assert run("""2 1
2 a 0 0
""") == "no"

# push-pop stability
assert run("""1 6
0 a 0 0
0 b 0 0
0 a 0 0
1 0 0
1 0 0
2 a 0 0
""") == "yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn trống | không | xử lý ngăn xếp trống một cách an toàn | 
| đẩy xen kẽ | có/không | tính đúng đắn của các cập nhật đa số | 
| chữ cái lặp đi lặp lại | vâng | điều kiện đa số nghiêm ngặt | 
| chu kỳ push-pop | vâng | tính nhất quán của ngăn xếp + đồng bộ tần số | 

## Vỏ cạnh 

Một trường hợp quan trọng đang truy vấn một ngăn xếp trống. Việc triển khai kiểm tra rõ ràng kích thước trước khi truy cập bản đồ tần số. Nếu không có bộ bảo vệ này, một phép chia hoặc tra cứu ngây thơ có thể coi các khóa bị thiếu là 0 một cách không chính xác và vẫn tiếp tục, điều này chỉ đúng nếu kích thước được xử lý cẩn thận. 

Một trường hợp khác là các thao tác pop lặp lại trên các ngăn xếp trống. Vì hoạt động phải ở trạng thái no-op nên việc triển khai sẽ tránh tràn dưới mức bằng cách kiểm tra`size[x][y] == 0`trước khi bật lên. Điều này đảm bảo rằng bản đồ ngăn xếp và tần số không bao giờ khác nhau. 

Trường hợp khó phát hiện cuối cùng là khi một chữ cái biến mất hoàn toàn khỏi chồng. Việc xóa các mục từ điển khi số đếm đạt đến 0 sẽ ngăn các khóa cũ khỏi logic gây hiểu lầm trong tương lai vốn cho rằng sự hiện diện của khóa hàm ý tính tích cực.
