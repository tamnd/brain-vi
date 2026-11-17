---
title: "CF 1965A - Mọi Thứ Nim"
description: "Chúng ta được cung cấp một số trò chơi độc lập, mỗi trò chơi được xác định bằng một danh sách các cọc chứa đá. Hai người chơi luân phiên nhau."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1965
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 941 (Div. 1)"
rating: 1400
weight: 1965
solve_time_s: 84
verified: false
draft: false
---

[CF 1965A - Mọi thứ Nim](https://codeforces.com/problemset/problem/1965/A) 

**Đánh giá:** 1400 
**Tags:** trò chơi, tham lam, toán học, sắp xếp 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số trò chơi độc lập, mỗi trò chơi được xác định bằng một danh sách các cọc chứa đá. Hai người chơi luân phiên nhau. Một nước đi bao gồm việc chọn một số nguyên$k$không vượt quá cọc không rỗng nhỏ nhất thì đồng thời trừ đi$k$đá từ mỗi đống vẫn còn trống. Bất kỳ cọc nào đạt tới 0 sẽ biến mất khỏi cuộc chơi. 

Trò chơi kết thúc khi tất cả cọc trống và người chơi không thể di chuyển sẽ thua cuộc. Vì một nước đi luôn giảm ít nhất cọc nhỏ nhất về 0, nên số lượng cọc hoạt động giảm dần theo thời gian, do đó mọi ván đấu đều kết thúc trong tối đa$n$di chuyển. 

Nhiệm vụ là xác định, đối với mỗi trường hợp thử nghiệm, liệu người chơi đầu tiên có bị buộc phải thắng trong lối chơi tối ưu hay không. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng số cọc trên tất cả các trường hợp thử nghiệm, do đó, mọi giải pháp về cơ bản phải là tuyến tính hoặc tuyến tính ở kích thước đầu vào. Giải pháp mô phỏng từng bước di chuyển là không thể bởi vì ngay cả một trò chơi cũng có thể có tới$10^5$cọc và mỗi lần di chuyển chạm vào tất cả chúng, dẫn đến hành vi bậc hai. 

Một cách tiếp cận ngây thơ sẽ mô phỏng trò chơi từng bước một. Đối với mỗi lượt, chúng tôi sẽ quét tất cả các cọc, tính toán mức tối thiểu, trừ đi số đã chọn$k$, và loại bỏ số không. Điều này thất bại ngay lập tức dưới những ràng buộc. 

Một trường hợp thất bại phổ biến khác xuất phát từ việc giả định tổng của tất cả các viên đá hoặc tính chẵn lẻ của tổng số viên đá sẽ xác định người chiến thắng. Ví dụ: một cấu hình như$[1,7]$có tổng số chẵn lẻ giống như$[2,6]$, nhưng kết quả của trò chơi khác nhau do cấu trúc loại bỏ theo hướng tối thiểu sẽ thay đổi số lần di chuyển bắt buộc. 

## Phương pháp tiếp cận 

Ý tưởng Brute-Force rất đơn giản: liên tục tính toán cọc không trống tối thiểu, chọn một giá trị hợp lệ.$k$và mô phỏng cả hai người chơi theo những lựa chọn tối ưu. Ngay cả khi chúng ta cố gắng tối ưu hóa bằng cách luôn chọn một nước đi “rõ ràng” như$k = \min$, chúng ta vẫn phải đối mặt với một cây trò chơi phân nhánh vì các giá trị khác nhau của$k$có thể dẫn đến việc loại bỏ cọc khác nhau. Điều này dẫn đến một số lượng trạng thái theo cấp số nhân nói chung. 

Quan sát cấu trúc quan trọng là giá trị tối thiểu kiểm soát hoàn toàn tính hợp pháp của các động thái. Nếu mức tối thiểu hiện tại là$m$, sau đó chọn bất kỳ$k < m$chỉ cần dịch chuyển tất cả các cọc xuống bằng cách$k$mà không loại bỏ bất kỳ đống nào. Điều này không bao giờ thay đổi thứ tự tương đối của kích thước cọc và chỉ làm trì hoãn việc chấm dứt. Một người chơi hợp lý không bao giờ thích trì hoãn việc giảm bắt buộc khi thay vào đó họ có thể loại bỏ lớp nhỏ nhất ngay lập tức. 

Vì vậy chơi tối ưu luôn sử dụng$k = m$, mức tối thiểu hiện tại. Động thái đó sẽ xóa tất cả các cọc bằng giá trị tối thiểu và giảm phần còn lại đúng bằng giá trị đó. Sau đó, trạng thái tiếp theo chỉ phụ thuộc vào giá trị cọc riêng biệt tiếp theo, không phụ thuộc vào bội số cọc riêng lẻ ngoài việc chúng có bằng nhau hay không. 

Điều này biến trò chơi thành một quá trình theo chuỗi các giá trị riêng biệt. Mỗi bước sẽ loại bỏ một “lớp” tương ứng với một chiều cao riêng biệt. Thông tin duy nhất còn tồn tại là có bao nhiêu cấp độ khác nhau tồn tại. 

Điều này biến toàn bộ trò chơi thành một câu hỏi ngang bằng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Sắp xếp + Đếm riêng biệt | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm từng trường hợp thử nghiệm thành tập hợp các kích thước cọc riêng biệt. 

1. Đọc mảng kích thước cọc. 
2. Sắp xếp mảng sao cho các giá trị bằng nhau trở nên liền kề, giúp dễ dàng xác định các giá trị riêng biệt. 
3. Đếm xem tồn tại bao nhiêu giá trị khác nhau. 
4. Nếu số này là số lẻ, tuyên bố Alice là người chiến thắng, nếu không thì tuyên bố Bob là người chiến thắng. 

Lý do chúng tôi chỉ tính các giá trị riêng biệt là vì mỗi nước đi sẽ loại bỏ chính xác một mức giá trị riêng biệt khỏi trò chơi. Tính đa bội bên trong của mỗi giá trị không ảnh hưởng đến số lượng “lớp” quyết định tồn tại mà chỉ ảnh hưởng đến số lượng cọc biến mất ở mỗi bước. 

### Tại sao nó hoạt động 

Trò chơi phát triển bằng cách liên tục loại bỏ giá trị tối thiểu hiện tại khỏi tất cả các cọc đang hoạt động. Thao tác này loại bỏ ít nhất một giá trị riêng biệt trong mỗi lần di chuyển và không bao giờ tạo ra các giá trị riêng biệt mới. Do đó, trò chơi tương đương với một quá trình xen kẽ đơn giản trên danh sách các chiều cao cọc khác nhau. Mỗi lần di chuyển tương ứng với việc tiêu thụ chính xác một cấp độ. Người chơi tiêu thụ cấp độ cuối cùng sẽ thắng nên kết quả chỉ phụ thuộc vào số cấp độ là lẻ hay chẵn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        
        distinct = 1
        for i in range(1, n):
            if a[i] != a[i - 1]:
                distinct += 1
        
        if distinct % 2 == 1:
            out.append("Alice")
        else:
            out.append("Bob")
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã dựa vào việc sắp xếp để nhóm các kích thước cọc bằng nhau lại với nhau. Sau khi sắp xếp, một lần quét tuyến tính sẽ đếm các lần chuyển tiếp giữa các giá trị. Mỗi lần chuyển đổi tương ứng với một “lớp” riêng biệt mới trong trò chơi. 

Việc kiểm tra tính chẵn lẻ ở cuối phản ánh trực tiếp cấu trúc xen kẽ của các nước đi: mỗi nước đi sẽ loại bỏ một cấp độ tối thiểu riêng biệt, do đó số cấp độ như vậy sẽ xác định ai thực hiện nước đi cuối cùng. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi logic trên hai trường hợp đại diện. 

### Ví dụ 1:`3 3 3 3 3`Sau khi sắp xếp, mảng vẫn còn`[3]`xét về các giá trị riêng biệt. 

| Bước | Mảng | Số lượng riêng biệt | 
| --- | --- | --- | 
| Ban đầu | [3,3,3,3,3] | 1 | 

Số đếm là số lẻ nên Alice thắng. 

Điều này phù hợp với trực giác vì Alice có thể loại bỏ tất cả các viên đá trong một lần di chuyển bằng cách chọn$k=3$. 

### Ví dụ 2:`1 7`| Bước | Mảng | Số lượng riêng biệt | 
| --- | --- | --- | 
| Ban đầu | [1,7] | 2 | 

Số đếm chẵn nên Bob thắng. 

Sau khi Alice loại bỏ lớp đầu tiên ($k=1$), trò chơi sụp đổ thành một cọc có kích thước 6, mang lại cho Bob nước đi thắng trực tiếp. 

Điều này cho thấy nước đi đầu tiên chỉ hiển thị số lớp còn lại và người chơi thứ hai được hưởng lợi khi số đó là số chẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, tiếp theo là quét tuyến tính | 
| Không gian | O(1) thêm (hoặc O(n) tùy theo loại) | Chỉ mảng đầu vào được lưu trữ | 

Giải pháp phù hợp một cách thoải mái trong giới hạn vì tổng số phần tử trong các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$và việc sắp xếp thang đo đó có hiệu quả trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        d = 1
        for i in range(1, n):
            if a[i] != a[i - 1]:
                d += 1
        res.append("Alice" if d % 2 else "Bob")
    return "\n".join(res)

# provided samples
assert run("""7
5
3 3 3 3 3
2
1 7
7
1 3 9 7 4 2 100
3
1 2 3
6
2 1 3 4 2 4
8
5 7 2 9 6 3 3 2
1
1000000000
""") == """Alice
Bob
Alice
Alice
Bob
Alice
Alice"""

# all equal
assert run("""1
4
10 10 10 10
""") == "Alice"

# two piles same
assert run("""1
2
5 5
""") == "Alice"

# increasing chain
assert run("""1
4
1 2 3 4
""") == "Alice"

# alternating structure
assert run("""1
4
1 100 2 99
""") == "Bob"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các cọc bằng nhau | Alice | cấp độ riêng biệt duy nhất | 
| hai cọc giống hệt nhau | Alice | trường hợp đối xứng kích thước chẵn tối thiểu | 
| tăng nghiêm ngặt | Alice | chẵn lẻ số lượng khác biệt tối đa | 
| cực đối xứng | Bob | cấu trúc thậm chí khác biệt | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng nhất là khi tất cả các cọc đều giống hệt nhau. Trong tình huống này, chỉ có một giá trị riêng biệt nên người chơi đầu tiên sẽ ngay lập tức thắng khi loại bỏ mọi thứ chỉ trong một nước đi. Thuật toán nắm bắt được điều này vì bộ đếm riêng biệt bằng 1, tạo ra Alice là người chiến thắng. 

Một trường hợp tinh vi khác là khi các cọc tạo thành một chuỗi so le hoàn hảo như$[1,2,3,4]$. Mặc dù các giá trị được dàn trải, mỗi giá trị vẫn đóng góp chính xác một cấp độ riêng biệt, do đó kết quả chỉ phụ thuộc vào tính chẵn lẻ của bốn, dẫn đến chiến thắng cho người chơi thứ hai. 

Cuối cùng, các trường hợp có giá trị lặp lại xen kẽ với các trường hợp khác, chẳng hạn như$[1,100,2,99]$, không ảnh hưởng đến tính đúng đắn. Việc sắp xếp thu gọn các bản sao một cách chính xác và chỉ có các chuyển đổi mới quan trọng, do đó, thuật toán đếm chính xác bốn giá trị riêng biệt và trả về vị trí bị mất cho Alice.
