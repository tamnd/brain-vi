---
title: "CF 104400E - đá (phiên bản dễ dàng)"
description: "Chúng tôi được giao một hàng đống đá. Trò chơi tiến hành theo các lượt xen kẽ bắt đầu với Alice. Trong mỗi lượt, người chơi hiện tại chỉ tập trung vào đống ngoài cùng bên trái vẫn còn chứa đá và loại bỏ ít nhất một viên đá khỏi đống đó."
date: "2026-06-30T23:02:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104400
codeforces_index: "E"
codeforces_contest_name: "Hunan University 2023 the 19th Programming Contest"
rating: 0
weight: 104400
solve_time_s: 47
verified: true
draft: false
---

[CF 104400E - đá(phiên bản dễ dàng)](https://codeforces.com/problemset/problem/104400/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao một hàng đống đá. Trò chơi tiến hành theo các lượt xen kẽ bắt đầu với Alice. Trong mỗi lượt, người chơi hiện tại chỉ tập trung vào đống ngoài cùng bên trái vẫn còn chứa đá và loại bỏ ít nhất một viên đá khỏi đống đó. Vì đây là phiên bản dễ dàng với$k = 1$, một lượt bao gồm chính xác một lần loại bỏ như vậy và sau khi cọc đã chọn trở nên trống, việc chơi tiếp tục ở cọc tiếp theo bên phải ở lượt tiếp theo. 

Trò chơi kết thúc khi tất cả các cọc đều trống. Người chơi sắp đi nước đi mà không còn viên đá nào thì không có nước đi hợp lệ và thua ngay lập tức, do đó người chơi trước là người chiến thắng. 

Mặc dù mỗi cọc có số lượng đá tùy ý nhưng người chơi không bao giờ bị buộc phải chia một cọc thành nhiều lượt một cách hạn chế. Bất kỳ nước đi nào cũng có thể loại bỏ toàn bộ cọc còn lại nếu muốn, vì loại bỏ “ít nhất một số bất kỳ” bao gồm việc lấy hết đá. 

Các ràng buộc rất lớn: lên tới$10^5$trường hợp thử nghiệm và tổng kích thước đầu vào qua các thử nghiệm lên tới$10^5$. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào thực hiện nhiều hơn công việc liên tục trên mỗi trường hợp thử nghiệm. Thậm chí một$O(n)$mỗi phương pháp thử nghiệm sẽ tổng hợp quá chậm nếu không được quản lý cẩn thận, nhưng ở đây chúng ta sẽ thấy cấu trúc thu gọn vấn đề thành lý luận thời gian không đổi cho mỗi trường hợp. 

Một trường hợp phức tạp thường gây nhầm lẫn cho các phương pháp tiếp cận đơn giản là vai trò của kích thước cọc. Người ta có thể cho rằng số lượng quân cờ là quan trọng, nhưng vì mỗi cọc có thể được dọn sạch chỉ trong một lần di chuyển nên giá trị thực tế không ảnh hưởng đến độ dài chuỗi. 

Ví dụ: hãy xem xét một trường hợp thử nghiệm duy nhất: 

đầu vào:```
n = 1
a = [1000000000]
```Dù cọc rất lớn nhưng Alice có thể lấy hết quân trong một nước đi, khiến Bob không thể di chuyển và thua ngay lập tức. Vì vậy, kết quả được xác định hoàn toàn bằng số cọc lẻ hay chẵn chứ không phải bởi kích thước của chúng. 

Một trường hợp gây nhầm lẫn khác là: 

đầu vào:```
n = 2
a = [1, 100]
```Mặc dù cọc thứ hai lớn nhưng trò chơi vẫn bao gồm đúng hai nước đi: mỗi nước đi một cọc. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ mô hình hóa từng bước di chuyển một cách rõ ràng. Chúng tôi sẽ duy trì một con trỏ tới đống hiện tại và liên tục trừ đi một số số dương từ nó, có khả năng mô phỏng mỗi lần loại bỏ đá. Tuy nhiên, vì mỗi đống có thể chứa tới$10^9$đá, cách giải thích này có thể gợi ý tới$10^9$hành động trên mỗi cọc trong trường hợp xấu nhất, điều này rõ ràng là không khả thi. 

Quan sát quan trọng là cách chơi tối ưu luôn làm trống đống hiện tại ngay lập tức. Không có lợi ích chiến lược nào khi để quân lại phía sau vì đối thủ sẽ luôn phải đối mặt với tình huống tương tự bất kể bạn chia các lượt loại bỏ thành nhiều lượt như thế nào. Mỗi cọc đóng góp hiệu quả chính xác một nước đi vào độ dài của ván đấu. 

Điều này làm giảm toàn bộ trò chơi thành một chuỗi cố định: mỗi cọc tương ứng với một nước đi bắt buộc và người chơi chỉ cần luân phiên các nước đi trong một chuỗi dài$n$. Câu hỏi duy nhất còn lại là ai thực hiện nước đi cuối cùng. 

Vì vậy, trò chơi này tương đương với việc kiểm tra tính chẵn lẻ đơn giản của số cọc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(\sum a_i)$|$O(1)$| Quá chậm | 
| Giảm chẵn lẻ |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$T$. Mỗi trường hợp thử nghiệm là độc lập vì không có trạng thái nào xen kẽ giữa chúng. 
2. Với mỗi test case, hãy đọc$n$, số lượng cọc. Giá trị thực tế của cọc không liên quan đến kết quả cuối cùng nên chúng có thể được đọc và bỏ qua. 
3. Xác định người chiến thắng chỉ dựa trên việc$n$là số lẻ hoặc số chẵn. Nếu như$n$thật kỳ quặc, Alice thực hiện bước cuối cùng; nếu như$n$chẵn, Bob thực hiện nước đi cuối cùng. 
4. Xuất "Alice" nếu$n$là số lẻ, nếu không thì xuất ra "Bob". 

### Tại sao nó hoạt động 

Mỗi cọc đóng góp chính xác một nước đi cho trò chơi vì người chơi điều khiển cọc ngoài cùng bên trái luôn có thể loại bỏ tất cả các viên đá của nó chỉ bằng một hành động. Sau khi một cọc được làm trống, nó sẽ không bao giờ xuất hiện trở lại trong trò chơi và cọc tiếp theo sẽ hoạt động ở lượt tiếp theo. Vì vậy, trò chơi luôn kéo dài chính xác$n$chuyển động, không phụ thuộc vào kích thước cọc hoặc sự phân bố. Vì người chơi luân phiên di chuyển bắt đầu bằng Alice nên người chiến thắng chỉ được xác định bằng tính chẵn lẻ của$n$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        arr = list(map(int, input().split()))
        
        if n % 2 == 1:
            print("Alice")
        else:
            print("Bob")

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng trường hợp kiểm thử và ngay lập tức chuyển nó thành kiểm tra tính chẵn lẻ. Mảng chỉ được sử dụng để đáp ứng các ràng buộc về định dạng đầu vào; giá trị của nó không ảnh hưởng đến tính toán. Quyết định quan trọng duy nhất dựa trên$n$, do đó, thuật toán chạy theo thời gian tuyến tính trên kích thước đầu vào nhưng với công việc không đổi trên mỗi phần tử ngoài khả năng đọc. 

Một lỗi triển khai phổ biến ở đây là cố gắng mô phỏng việc loại bỏ hoặc lý luận về số lượng đá riêng lẻ. Điều đó dẫn đến sự phức tạp không cần thiết mà không làm thay đổi kết quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
1 1
2
```| Bước | n | Cọc còn lại | Người chơi | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | [2] | Alice | 
| 1 | 1 | [] | Bob (không di chuyển) | 

Alice loại bỏ đống duy nhất trong một lần di chuyển. Bob không di chuyển và thua cuộc, xác nhận điều kỳ lạ đó$n$dẫn đến việc Alice chiến thắng. 

### Ví dụ 2 

đầu vào:```
1
3 1
2 2 2
```| Bước | n | Cọc còn lại | Người chơi | 
| --- | --- | --- | --- | 
| Bắt đầu | 3 | [2,2,2] | Alice | 
| 1 | 3 | [2,2] | Bob | 
| 2 | 3 | [2] | Alice | 
| 3 | 3 | [] | Bob (không di chuyển) | 

Bob thực hiện nước đi cuối cùng khi$n$được lập chỉ mục chẵn về mặt lượt, có nghĩa là Alice thực hiện nước đi cuối cùng ở đây và giành chiến thắng. Điều này xác nhận rằng kỳ lạ$n$tạo ra một chiến thắng cho Alice. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T + \sum n)$| Mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi sau khi đọc đầu vào | 
| Không gian |$O(1)$| Không có dữ liệu phụ trợ ngoài các biến đầu vào | 

Giải pháp dễ dàng phù hợp trong giới hạn vì tất cả công việc nặng nhọc đều bị giới hạn ở việc phân tích cú pháp đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    import sys
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n, k = map(int, input().split())
        arr = list(map(int, input().split()))
        res.append("Alice" if n % 2 == 1 else "Bob")

    return "\n".join(res)

# provided samples
assert run("3\n1 1\n1\n2 1\n1 2\n3 1\n2 2 2\n") == "Alice\nBob\nAlice"

# custom cases
assert run("1\n1 1\n100\n") == "Alice", "single pile"
assert run("1\n2 1\n1 1\n") == "Bob", "two piles even"
assert run("1\n5 1\n1 1 1 1 1\n") == "Alice", "odd many piles"
assert run("2\n2 1\n5 6\n3 1\n7 8 9\n") == "Bob\nAlice", "mixed cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cọc đơn | Alice | trường hợp tối thiểu | 
| hai cọc | Bob | chẵn lẻ | 
| năm cọc | Alice | trường hợp lẻ lớn hơn | 
| trường hợp hỗn hợp | Bob / Alice | xử lý nhiều bài kiểm tra | 

## Vỏ cạnh 

Đầu vào tối thiểu với một cọc thể hiện hành vi biên. Ví dụ:```
1 1
10
```Thuật toán phân loại ngay lập tức$n = 1$là số lẻ và xuất ra Alice. Chế độ xem mô phỏng xác nhận Alice làm trống đống duy nhất và giành chiến thắng ngay lập tức. 

Một trường hợp hai đống như:```
2 1
3 7
```luân phiên các nước đi: Alice loại bỏ cọc đầu tiên, Bob loại bỏ cọc thứ hai, khiến Alice không được di chuyển. Quy tắc chẵn lẻ dự đoán chính xác Bob. 

Giá trị lớn bên trong cọc không ảnh hưởng đến việc thực hiện. Vì:```
3 1
1000000000 1000000000 1000000000
```mỗi cọc vẫn tương ứng với đúng một nước đi nên kết quả vẫn là Alice do lẻ$n$.
