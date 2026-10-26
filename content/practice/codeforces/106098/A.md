---
title: "CF 106098A - Hói và du lịch"
description: "Bài toán là một trò chơi dành cho hai người chơi trên một danh sách các bài toán, trong đó mỗi bài toán có một giá trị độ khó riêng biệt. Các giá trị độ khó tạo thành một hoán vị nên mỗi số từ 1 đến n xuất hiện đúng một lần. Khách du lịch di chuyển trước và người chơi thay phiên nhau di chuyển."
date: "2026-06-25T11:54:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106098
codeforces_index: "A"
codeforces_contest_name: "The American University in Cairo CSEA Fall 2025 contest"
rating: 0
weight: 106098
solve_time_s: 38
verified: true
draft: false
---

[CF 106098A - Hói và Khách du lịch](https://codeforces.com/problemset/problem/106098/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán là một trò chơi dành cho hai người chơi trên một danh sách các bài toán, trong đó mỗi bài toán có một giá trị độ khó riêng biệt. Các giá trị độ khó tạo thành một hoán vị nên mỗi số từ 1 đến n xuất hiện đúng một lần. 

Khách du lịch di chuyển trước và người chơi thay phiên nhau di chuyển. Ở mỗi lượt, người chơi hiện tại chọn bất kỳ vấn đề nào còn lại và ngay lập tức nhận được số điểm tương ứng với độ khó của nó. Vấn đề đó sau đó được loại bỏ khỏi nhóm. Trò chơi kết thúc khi không còn vấn đề gì và người chiến thắng là người chơi có tổng điểm lớn hơn. Nếu cả hai điểm bằng nhau, Du khách vẫn không thua một cách rõ ràng, nhưng nhiệm vụ yêu cầu chúng ta quyết định xem ai thắng theo cách chơi tối ưu. 

Một quan sát quan trọng là không có hạn chế nào về việc có thể giải quyết vấn đề nào vào bất kỳ lúc nào, vì vậy cấu trúc duy nhất xuất phát từ quy tắc tính điểm và thứ tự lần lượt. Vì tất cả các giá trị đều dương và khác biệt nên cả hai người chơi luôn được khuyến khích lấy các giá trị còn lại lớn khi có thể. 

Kích thước đầu vào lên tới 100000, điều này ngay lập tức loại trừ mọi giải pháp mô phỏng các chiến lược liên quan đến tìm kiếm hoặc tính toán lại các lựa chọn cho mỗi nước đi trong thời gian bậc hai. Bất kỳ phương pháp nào kiểm tra tất cả các phần tử còn lại nhiều lần sẽ giảm xuống O(n^2), quá chậm. Chúng ta nên kỳ vọng rằng câu trả lời phụ thuộc vào thuộc tính tổ hợp trực tiếp của hoán vị hơn là bất kỳ mô phỏng động nào. 

Trường hợp cạnh tinh tế xuất hiện khi n nhỏ hoặc khi thứ tự đối nghịch. 

Ví dụ: khi n = 1 và mảng là [1], Du lịch lấy phần tử duy nhất và thắng. 

Khi n = 2 và mảng là [1, 2], Du lịch lấy 2, Hói lấy 1 và Du lịch thắng 1 điểm. Một ý tưởng ngây thơ có thể cho rằng chỉ riêng thứ tự lần lượt đã xác định người chiến thắng, nhưng điều này đã gây hiểu lầm vì cách chơi tối ưu luôn chọn phần tử lớn nhất còn lại. 

Vấn đề sâu xa hơn là bất kỳ chiến lược nào cố gắng theo dõi “vị trí” trong mảng đều không liên quan vì mảng đó không có hiệu lực về vị trí, chỉ có nhiều tập hợp giá trị mới quan trọng. 

## Phương pháp tiếp cận 

Mô phỏng bạo lực sẽ cố gắng mô hình hóa trạng thái trò chơi một cách rõ ràng. Ở mỗi lượt, người chơi sẽ quét tất cả các phần tử còn lại, chọn một phần tử theo chiến lược nào đó và loại bỏ nó. Ngay cả khi cả hai người chơi luôn tham lam chọn, chúng tôi vẫn mô phỏng n lượt và mỗi lượt yêu cầu quét tối đa O(n) phần tử, dẫn đến thời gian O(n^2). Điều này đã thất bại với n = 100000. 

Sự đơn giản hóa quan trọng là nhận ra rằng cả hai người chơi đều cân xứng về năng lực và hoàn toàn hợp lý, và mọi nước đi chỉ đơn giản là sự lựa chọn của một số còn lại. Vì tất cả các giá trị đều khác biệt và bổ sung, nên quyết định có ý nghĩa duy nhất là thứ tự sử dụng nhiều tập hợp giá trị. 

Nếu chúng ta sắp xếp tất cả các giá trị theo thứ tự giảm dần, cách chơi tối ưu sẽ chỉ định giá trị lớn nhất còn lại cho người chơi hiện tại trong lượt của họ một cách hiệu quả. Không có trường hợp nào mà người chơi bỏ qua giá trị lớn hơn để lấy giá trị nhỏ hơn, bởi vì điều đó chỉ làm giảm tổng cuối cùng của họ và mang lại cho đối thủ một vị trí tốt hơn trong tương lai. 

Do đó trò chơi giảm xuống việc phân phối các giá trị được sắp xếp luân phiên giữa Du lịch và Hói. Khách du lịch được chọn đầu tiên, sau đó là Hói, v.v. 

Vậy Du lịch nhận các giá trị tại vị trí 0, 2, 4, ... trong mảng đã sắp xếp và Bald nhận các giá trị tại vị trí 1, 3, 5, ... 

Người chiến thắng được xác định bằng cách so sánh tổng của hai chuỗi con này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Sắp xếp + Tổng xen kẽ | O(n log n) | O(1) bổ sung (ngoài đầu vào) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc hoán vị của kích thước n và lưu trữ tất cả các giá trị. 
2. Sắp xếp các giá trị theo thứ tự giảm dần để chúng ta luôn xem xét lựa chọn tốt nhất còn lại trước tiên. 
3. Khởi tạo hai bộ tích lũy, một cho Du lịch và một cho Hói. 
4. Lặp lại mảng đã được sắp xếp. Gán chỉ số 0, 2, 4, ... cho Du lịch và 1, 3, 5, ... cho Bald, cộng từng giá trị vào tổng tương ứng. 
5. So sánh hai tổng. Nếu tổng số tiền của Du khách lớn hơn thì anh ta sẽ thắng. Nếu không thì Hói sẽ thắng. 

Bước lý luận quan trọng là tại sao việc phân công xen kẽ lại hợp lệ. Tại bất kỳ thời điểm nào trong trò chơi, cả hai người chơi đều nhìn thấy bộ còn lại giống nhau. Vì tất cả các giá trị đều dương và độc lập nên việc lấy một giá trị nhỏ hơn trong khi tồn tại một giá trị lớn hơn không bao giờ có thể cải thiện được kết quả. Điều này buộc cả hai người chơi đều có hành vi tham lam giống nhau, khiến cho nhiệm vụ luân phiên được sắp xếp thể hiện kết quả tối ưu duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    a.sort(reverse=True)
    
    tourist = 0
    bald = 0
    
    for i, val in enumerate(a):
        if i % 2 == 0:
            tourist += val
        else:
            bald += val
    
    if tourist > bald:
        print("Tourist")
    else:
        print("Bald")

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sắp xếp mảng theo thứ tự giảm dần để các lựa chọn sẵn có mạnh nhất luôn được xem xét trước tiên. Sau đó, vòng lặp chỉ định các giá trị hoàn toàn dựa trên chỉ số chẵn lẻ của lượt chơi, tương ứng với người chơi trước và duy trì sự luân phiên nghiêm ngặt. 

Một lỗi phổ biến ở đây là cố gắng mô phỏng các quyết định thực tế của trò chơi bằng hàng đợi ưu tiên nhưng vẫn tính toán lại các lựa chọn mỗi lượt một cách không cần thiết. Điều đó dẫn đến logic dư thừa mà không làm thay đổi kết quả. Cái nhìn sâu sắc chính xác là trò chơi không bao giờ đi chệch khỏi việc phân công thứ tự giảm dần trên toàn cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 3 2
```Mảng được sắp xếp là [3, 2, 1]. 

| Bước | Giá trị được chọn | Tổng số du lịch | Tổng Hói | 
| --- | --- | --- | --- | 
| 0 | 3 | 3 | 0 | 
| 1 | 2 | 3 | 2 | 
| 2 | 1 | 4 | 2 | 

Du lịch kết thúc với 4, Bald với 2, vì vậy Du lịch thắng. 

Ví dụ này cho thấy việc tiếp cận sớm với giá trị lớn nhất sẽ chi phối kết quả như thế nào ngay cả khi các giá trị sau này được cân bằng. 

### Ví dụ 2 

đầu vào:```
4
4 1 7 2
```Mảng được sắp xếp là [7, 4, 2, 1]. 

| Bước | Giá trị được chọn | Tổng số du lịch | Tổng Hói | 
| --- | --- | --- | --- | 
| 0 | 7 | 7 | 0 | 
| 1 | 4 | 7 | 4 | 
| 2 | 2 | 9 | 4 | 
| 3 | 1 | 9 | 5 | 

Du khách thắng với tỉ số 9 vs 5. 

Điều này xác nhận rằng việc phân công tham lam xen kẽ luôn mô hình hóa cách chơi tối ưu bất kể thứ tự ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, chuyển tuyến tính đơn sau | 
| Không gian | O(1) thêm | Chỉ có hai bộ tích lũy ngoài bộ nhớ đầu vào | 

Các ràng buộc cho phép tối đa 100000 phần tử và việc sắp xếp ở quy mô này nằm trong giới hạn một cách thoải mái. Đường truyền tuyến tính không đáng kể so với cách sắp xếp và việc sử dụng bộ nhớ vẫn ở mức tối thiểu do không cần có cấu trúc bổ sung nào ngoài mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n = int(input())
    a = list(map(int, input().split()))
    
    a.sort(reverse=True)
    
    t = 0
    b = 0
    
    for i, v in enumerate(a):
        if i % 2 == 0:
            t += v
        else:
            b += v
    
    return "Tourist" if t > b else "Bald"

# provided sample
assert run("3\n1 3 2\n") == "Tourist"

# minimum size
assert run("1\n1\n") == "Tourist"

# small even case
assert run("2\n1 2\n") == "Tourist"

# all increasing
assert run("5\n1 2 3 4 5\n") == "Tourist"

# all large spread
assert run("4\n10 1 100 2\n") == "Tourist"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | Du lịch | trường hợp cơ sở | 
| 2 yếu tố | Du lịch | lợi thế nước đi đầu tiên | 
| trình tự tăng dần | Du lịch | sắp xếp đúng đắn | 
| giá trị hỗn hợp | Du lịch | tính nhất quán phân công tham lam | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là n = 1. Với đầu vào`1 1`, Du lịch ngay lập tức lấy giá trị duy nhất và thắng. Thuật toán gán chỉ số 0 cho Du lịch, do đó Du lịch nhận được toàn bộ số tiền một cách chính xác. 

Một trường hợp cạnh khác là khi giá trị lớn nhất lớn hơn nhiều so với tổng của tất cả các giá trị khác, chẳng hạn`100000 1 2 3 ...`. Sau khi sắp xếp, Du lịch luôn lấy phần tử lớn nhất trước tiên, đảm bảo sự thống trị bất kể phân phối còn lại. Phép gán xen kẽ phản ánh chính xác điều này vì phần tử đầu tiên luôn được gán cho Du lịch. 

Trường hợp khó phát hiện cuối cùng là khi tổng rất gần nhau. Ví dụ`3 2 1`. Quá trình sắp xếp mang lại Du lịch = 3 + 1 và Bald = 2, do đó Du lịch vẫn thắng. Điều này xác nhận rằng phân bổ dựa trên tính chẵn lẻ mô hình chính xác hành vi tối ưu ngay cả khi trực giác ngây thơ có thể gợi ý các động thái cân bằng.
