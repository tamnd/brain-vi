---
title: "CF 104846C - \u041f\u043e\u0438\u0441\u043a \u0441\u043e\u043a\u0440\u043e\u0432\u0438\u0449"
description: "Chúng tôi được cấp một số rương, mỗi rương chứa một số xu nhất định. Hai người bạn muốn chia xu để mỗi rương cuối cùng đóng góp như nhau cho cả hai, nhưng một rương chỉ có thể được “rút ra tiền” nếu số xu của nó là chẵn."
date: "2026-06-28T11:27:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104846
codeforces_index: "C"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 2023-2024 (7-8 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 104846
solve_time_s: 59
verified: true
draft: false
---

[CF 104846C - \u041f\u043e\u0438\u0441\u043a \u0441\u043e\u043a\u0440\u043e\u0432\u0438\u0449](https://codeforces.com/problemset/problem/104846/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số rương, mỗi rương chứa một số xu nhất định. Hai người bạn muốn chia xu để mỗi rương cuối cùng đóng góp như nhau cho cả hai, nhưng một rương chỉ có thể được “rút ra tiền” nếu số xu của nó là chẵn. Nếu một rương có số xu lẻ, chúng ta được phép di chuyển tất cả xu từ rương này sang rương khác, hợp nhất các cọc một cách hiệu quả cho đến khi chúng ta quyết định xử lý chúng. 

Khi một chiếc rương được xử lý, số xu của nó sẽ được chia đều cho hai người bạn, do đó, một chiếc rương có$x$tiền xu đóng góp$x/2$xu cho mỗi người, nhưng chỉ khi$x$ngay cả tại thời điểm xử lý. 

Nhiệm vụ không phải là đưa ra một chuỗi các bước di chuyển mà là tính toán số xu tối đa mà mỗi người bạn có thể kiếm được sau khi hợp nhất các rương và xử lý chúng một cách tối ưu. 

Mặc dù tuyên bố nói về các rương riêng lẻ, điểm tự do chính về cấu trúc là chúng ta có thể liên tục di chuyển toàn bộ cọc, nghĩa là chúng ta có thể sắp xếp lại các đồng xu một cách tùy ý thành các nhóm mới một cách hiệu quả trước khi quyết định chia tách đồng xu nào. 

Từ góc độ ràng buộc, rõ ràng chúng ta đang ở trong một chế độ tuyến tính: chỉ có tổng và tính chẵn lẻ của các giá trị là quan trọng. Bất kỳ giải pháp nào cố gắng mô phỏng các lựa chọn hợp nhất một cách rõ ràng sẽ trở thành phương trình bậc hai hoặc tệ hơn và sẽ thất bại ngay lập tức một lần.$n$lớn lên. 

Một sai lầm phổ biến là nghĩ rằng các rương riêng lẻ có sự đóng góp độc lập. Ví dụ, người ta có thể cố gắng xử lý các rương chẵn trước và bỏ qua các rương lẻ. Điều này phá vỡ trong các trường hợp như$1, 1, 8$, trong đó việc hợp nhất các rương lẻ trước tiên sẽ tăng khối lượng chẵn có thể sử dụng được. 

Một cạm bẫy khác là cho rằng những chiếc rương lẻ vốn đã vô dụng. Ví dụ, trong đầu vào$3, 1, 1$, hai rương lẻ có thể được hợp nhất thành$2$, làm cho chúng hoàn toàn có thể sử dụng được. Điều trị từng ngực một cách độc lập sẽ làm mất tác dụng này. 

## Phương pháp tiếp cận 

Một cách giải thích vũ phu sẽ thử mọi cách để hợp nhất các rương thành các nhóm và sau đó quyết định nhóm nào trở nên đồng đều và được xử lý. Mỗi nhóm thay đổi tổng số tiền và đối với mỗi nhóm, chúng tôi sẽ tính toán số lượng xu có thể được phân phối. Số lượng phân vùng của$n$các mục tăng theo cấp số nhân và thậm chí hạn chế chúng tôi hợp nhất theo cặp dẫn đến không gian tìm kiếm theo cấp số nhân. Cách tiếp cận này nhanh chóng trở nên không khả thi một khi$n$vượt quá một con số nhỏ. 

Quan sát quan trọng là việc hợp nhất sẽ loại bỏ hoàn toàn cấu trúc: chúng ta không bao giờ bị giới hạn bởi rương ban đầu mà đồng xu đến từ đâu. Bất kỳ chuỗi hợp nhất nào cũng cho phép chúng ta sắp xếp lại các đồng xu thành các chồng tùy ý, nghĩa là bất biến thực sự duy nhất của hệ thống là tổng số tiền của tất cả các đồng xu và liệu chúng ta có thể loại bỏ một đồng xu còn sót lại khi tổng là số lẻ hay không. 

Khi chúng tôi xem quy trình theo cách này, vấn đề sẽ không còn là về các rương mà trở thành vấn đề có thể ghép được bao nhiêu đồng xu. Mọi hoạt động đều cho phép chúng tôi sắp xếp lại các đồng xu một cách hiệu quả để tất cả trừ một đồng xu có thể tham gia vào việc chia đều hợp lệ. Điều đó làm giảm vấn đề tối đa hóa số lượng cặp tiền đầy đủ mà chúng ta có thể hình thành trên toàn cầu. 

Điều này trực tiếp dẫn đến kết luận rằng mỗi người bạn nhận được chính xác một nửa số xu ngoại trừ có thể có một đồng xu còn sót lại không thể ghép đôi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Nhóm và hợp nhất Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Giảm tổng + chẵn lẻ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý dữ liệu đầu vào một lần và tính toán tổng số xu trên tất cả các rương. 

1. Tính tổng của tất cả$a_i$. Điều này thể hiện tổng số lượng vật liệu có sẵn để chia tách. Không có hoạt động hợp nhất nào làm thay đổi tổng này, vì vậy đây là số lượng toàn cầu duy nhất quan trọng. 
2. Quan sát tính chẵn lẻ của tổng. Nếu tổng số là chẵn thì mỗi đồng xu có thể được ghép với một đồng xu khác theo một cách sắp xếp nào đó, vì vậy tất cả các đồng xu đều góp phần hoàn toàn vào việc chia tách. 
3. Nếu tổng số là số lẻ, chính xác một đồng xu chắc chắn sẽ không được ghép đôi bất kể chúng ta hợp nhất các rương như thế nào, bởi vì việc hợp nhất sẽ duy trì tổng số chẵn lẻ. Đồng xu duy nhất đó không thể là một phần của bất kỳ sự phân chia đồng đều hợp lệ nào và thực sự trở nên không thể sử dụng được. 
4. Câu trả lời cuối cùng là một nửa số tiền có thể sử dụng được, nghĩa là$\lfloor \text{sum} / 2 \rfloor$. 

### Tại sao nó hoạt động 

Việc hợp nhất cho phép phân phối lại tiền xu một cách tùy ý, do đó việc nhận dạng các rương ban đầu là không liên quan. Hạn chế duy nhất tồn tại trong tất cả các hoạt động là tính chẵn lẻ: việc chia tách yêu cầu tổng số chẵn và mỗi lần chia hợp lệ sẽ tiêu thụ số xu theo cặp. Vì việc ghép nối mang tính toàn cầu và không bị hạn chế nên kết quả tốt nhất có thể là ghép càng nhiều đồng tiền càng tốt trên toàn bộ nhiều bộ. Nhiều nhất một đồng xu có thể vẫn chưa ghép đôi, vì vậy khối lượng sử dụng tối đa là số chẵn lớn nhất không vượt quá tổng số tiền. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

total = sum(a)
print(total // 2)
```Việc thực hiện phản ánh trực tiếp việc giảm thiểu. Tổng được tích lũy một lần và phép chia số nguyên cho hai sẽ tự động xử lý cả trường hợp chẵn và lẻ mà không cần kiểm tra tính chẵn lẻ rõ ràng. 

Một điểm tinh tế là không cần mô phỏng việc hợp nhất. Mặc dù tuyên bố nhấn mạnh đến việc di chuyển toàn bộ rương, hoạt động này chỉ nhằm mục đích biện minh rằng việc tập hợp lại tùy ý là có thể thực hiện được chứ không yêu cầu thực hiện rõ ràng. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào như:$1, 8, 2$Chúng tôi theo dõi tổng số tiền và kết quả trả lời. 

| Bước | Trạng thái mảng | Tổng số tiền | Giải thích | 
| --- | --- | --- | --- | 
| 1 | [1, 8, 2] | 11 | Tất cả tiền được thu thập | 
| 2 | hợp nhất về mặt khái niệm | 11 | Được phép phân phối lại | 
| 3 | cuối cùng | 5 | tầng(2/11) | 

Điều này cho thấy rằng mặc dù rương đầu tiên là lẻ và không thể sử dụng được một mình, nó vẫn góp phần thông qua việc hợp nhất. 

Bây giờ hãy xem xét:$3, 1, 1$| Bước | Trạng thái mảng | Tổng số tiền | Giải thích | 
| --- | --- | --- | --- | 
| 1 | [3, 1, 1] | 5 | Có hai giá trị lẻ | 
| 2 | tỷ lệ cược hợp nhất | 5 | có thể hình thành chẵn + đồng xu còn sót lại | 
| 3 | cuối cùng | 2 | tầng(5/2) | 

Điều này chứng tỏ rằng các yếu tố lẻ sẽ không bị lãng phí nếu chúng có thể được ghép nối thông qua việc hợp nhất, nhưng một đồng tiền vẫn có thể không có đối thủ trên toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lần để tính tổng | 
| Không gian | O(1) | chỉ sử dụng ắc quy | 

Thuật toán này tối ưu cho thang đo đầu vào vì bất kỳ giải pháp nào ít nhất cũng phải đọc tất cả các giá trị, điều này vốn đã tốn thời gian tuyến tính. Việc sử dụng bộ nhớ là không đổi vì không cần cấu trúc nào vượt quá tổng hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    return str(sum(a) // 2)

# provided samples (reconstructed format where needed)
assert run("3\n1 8 2\n") == "5"

# single chest
assert run("1\n10\n") == "5"

# all odd
assert run("3\n1 1 1\n") == "1"

# all even
assert run("4\n2 4 6 8\n") == "10"

# mixed case
assert run("5\n3 1 4 1 5\n") == str((3+1+4+1+5)//2)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 8 2 | 5 | chẵn lẻ hỗn hợp cơ bản | 
| 1 10 | 5 | phần tử đơn | 
| 3 1 1 1 | 1 | mọi cách xử lý kỳ quặc | 
| 4 2 4 6 8 | 10 | tất cả đều tổng hợp | 
| 5 3 1 4 1 5 | 7 | tính đúng đắn chung | 

## Vỏ cạnh 

Một đầu vào một ngực như$10$kiểm tra xem giải pháp có cố gắng yêu cầu hợp nhất vào một rương khác không. Ở đây thuật toán chỉ đơn giản tính toán$10 // 2 = 5$, phù hợp với sự phân chia duy nhất có thể. 

Một cấu hình hoàn toàn kỳ lạ như$1, 1, 1$kiểm tra xem tính chẵn lẻ còn sót lại có được xử lý trên toàn cầu thay vì trên mỗi rương hay không. Tổng là 3, do đó câu trả lời trở thành 1, phản ánh rằng một đồng xu không thể ghép đôi sau khi hợp nhất tối ưu. 

Một trường hợp chẵn lớn như$2, 4, 6, 8$xác nhận rằng không có ràng buộc nhân tạo nào được đưa ra bởi logic. Tổng là 20 và luôn có thể ghép đôi đầy đủ, tặng 10 cho mỗi người bạn.
