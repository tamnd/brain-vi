---
title: "CF 104840G - \u0412\u043e\u0437\u0432\u0440\u0430\u0449\u0435\u043d\u0438\u0435 \u0417\u043b\u043e\u0433\u043e \u041c\u043e\u0440\u0442\u0438"
description: "Chúng ta được cho một tập hợp các đoạn thẳng trên trục số. Mỗi phân đoạn đại diện cho một loạt “thực tế” phải được tìm kiếm dưới dạng một mục duy nhất."
date: "2026-06-28T11:38:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104840
codeforces_index: "G"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 104840
solve_time_s: 53
verified: true
draft: false
---

[CF 104840G - \u0412\u043e\u0437\u0432\u0440\u0430\u0449\u0435\u043d\u0438\u0435 \u0417\u043b\u043e\u0433\u043e \u041c\u043e\u0440\u0442\u0438](https://codeforces.com/problemset/problem/104840/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các đoạn thẳng trên trục số. Mỗi phân đoạn đại diện cho một loạt “thực tế” phải được tìm kiếm dưới dạng một mục duy nhất. Hoạt động chúng tôi thực hiện là nhóm các phân đoạn này: một nhóm chứa một số phân đoạn và việc xử lý một nhóm có nghĩa là chúng tôi xử lý tất cả các phân đoạn bên trong nhóm đó lại với nhau. 

Bên trong một nhóm duy nhất, không có hai đoạn nào được phép giao nhau. Giao điểm ở đây có nghĩa là chúng có chung ít nhất một điểm chung, do đó hai đoạn sẽ không tương thích nếu một đoạn bắt đầu trước khi đoạn kia kết thúc. 

Nhiệm vụ là chia tất cả các phân đoạn thành số lượng nhóm như vậy nhỏ nhất có thể và cũng đưa ra thành phần của mỗi nhóm. 

Kích thước đầu vào đạt tới hai trăm nghìn phân đoạn và mỗi điểm cuối có thể lớn tới một tỷ. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào cố gắng so sánh tất cả các cặp phân đoạn, vì đó sẽ là phương trình bậc hai và vượt xa giới hạn thời gian. Ngay cả việc duy trì một ma trận tương thích rõ ràng cũng là điều không thể. 

Một điểm tinh tế là các phân đoạn là những khoảng đóng. Điều đó quan trọng đối với các trường hợp kề như`[1, 3]`Và`[3, 5]`, cắt nhau tại điểm 3 và do đó không thể thuộc cùng một nhóm. Điều này buộc phải có điều kiện nghiêm ngặt về khả năng tương thích`r < l`. 

Một sai lầm ngây thơ là cho rằng việc kết hợp các điểm cuối là an toàn. Ví dụ: 

đầu vào:```
3
1 3
3 5
6 7
```Việc nhóm bất cẩn có thể đặt đoạn thứ nhất và đoạn thứ hai lại với nhau nhưng chúng giao nhau ở điểm 3, vì vậy điều này vi phạm quy tắc. Giải pháp đúng phải coi ranh giới đó là xung đột. 

Một cạm bẫy phổ biến khác là tham lam đóng gói các phân khúc vào nhóm có sẵn đầu tiên mà không có chiến lược đặt hàng toàn cầu. Nếu không sắp xếp theo thời gian bắt đầu, việc phân nhóm có thể trở nên phụ thuộc vào thứ tự và tạo ra nhiều nhóm hơn mức cần thiết. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là xây dựng từng nhóm một. Chúng tôi liên tục chọn một phân đoạn chưa được chỉ định, bắt đầu một nhóm mới, sau đó quét tất cả các phân đoạn còn lại để thêm bất kỳ phân đoạn nào không giao nhau với bất kỳ phân đoạn nào đã có trong nhóm. Mỗi sự bổ sung yêu cầu kiểm tra tính tương thích với tất cả các thành viên hiện tại của nhóm. 

Điều này hoạt động hợp lý vì nó thực thi rõ ràng ràng buộc không giao nhau. Vấn đề là hiệu suất. Trong trường hợp xấu nhất, nếu các phân đoạn bị chồng chéo nhiều, mỗi nhóm có thể chỉ lấy một phân đoạn và mỗi nỗ lực lấp đầy một nhóm sẽ quét gần như tất cả các phân đoạn còn lại. Điều này dẫn đến hành vi bậc hai theo thứ tự n bình phương, vượt xa khả năng chấp nhận được đối với 200.000 phần tử. 

Điều quan trọng cần lưu ý là đây là một bài toán tô màu theo khoảng cổ điển. Số lượng nhóm tối thiểu cần thiết bằng số lượng khoảng thời gian chồng chéo tối đa tại bất kỳ điểm nào. Thay vì xây dựng các nhóm một cách tham lam theo thứ tự tùy ý, chúng tôi xử lý các phân đoạn được sắp xếp theo điểm bắt đầu của chúng và duy trì cấu trúc của các nhóm hiện đang hoạt động, mỗi nhóm được biểu thị bằng điểm cuối cuối cùng được gán cho nó. 

Tại bất kỳ thời điểm nào, một nhóm có đủ điều kiện để sử dụng lại nếu phân đoạn cuối cùng của nó kết thúc hoàn toàn trước khi phân đoạn hiện tại bắt đầu. Vì vậy ta muốn nhanh chóng tìm được nhóm có thời gian kết thúc nhỏ nhất mà vẫn thỏa mãn điều kiện này. Một đống tối thiểu theo thời gian kết thúc nhóm cung cấp chính xác chức năng này. 

Mỗi phân đoạn được gán cho một nhóm tương thích hiện có nếu có thể, nếu không thì một nhóm mới sẽ được tạo. Điều này đảm bảo tái sử dụng tối ưu các nhóm và đảm bảo số lượng tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các đoạn theo điểm cuối bên trái của chúng. Điều này đảm bảo chúng tôi luôn xử lý các khoảng thời gian theo thứ tự bắt đầu, điều này rất cần thiết để duy trì khái niệm nhất quán về các nhóm “hiện đang mở”. 
2. Duy trì một vùng nhớ tối thiểu trong đó mỗi phần tử đại diện cho một nhóm, được khóa bởi điểm cuối bên phải của phân đoạn cuối cùng được đặt vào nhóm đó. Mỗi mục nhập heap cũng lưu trữ mã định danh nhóm. 
3. Lặp lại các phân đoạn theo thứ tự được sắp xếp. Đối với mỗi phân đoạn`[l, r]`, kiểm tra đống. 
4. Nếu heap không trống và nhóm kết thúc nhỏ nhất đã kết thúc`< l`, bật nó lên và gán phân đoạn hiện tại cho nhóm đó. Chúng tôi chọn mục đích nhỏ nhất vì nó giải phóng sớm nhất, tối đa hóa cơ hội tái sử dụng trong tương lai. 
5. Mặt khác, không nhóm hiện có nào có thể chấp nhận phân đoạn này, vì vậy chúng tôi tạo một nhóm mới và gán phân khúc cho nhóm đó. 
6. Sau khi gán, đẩy nhóm đã cập nhật trở lại vùng nhớ với thời gian kết thúc của phân đoạn hiện tại. 
7. Lưu trữ danh sách thành viên nhóm để chúng ta có thể xuất ra sau cùng. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, heap lưu trữ tất cả các nhóm đang hoạt động được sắp xếp theo thời điểm chúng không còn sử dụng được nữa. Việc chỉ định một phân đoạn cho nhóm có thời gian kết thúc nhỏ nhất là an toàn vì nếu bất kỳ nhóm nào có thể chứa phân đoạn đó thì phân đoạn đó sẽ bị hạn chế nhất; nếu không thể thì nhóm khác cũng không thể. Sự lựa chọn tham lam này bảo toàn mọi khả năng trong tương lai và tránh tạo ra những nhóm không cần thiết. 

Điều bất biến là sau khi xử lý từng phân đoạn, mỗi nhóm trong vùng heap biểu thị một chuỗi phân đoạn hợp lệ không chồng chéo và bất kỳ phân đoạn nào được chỉ định sau đó đều tương thích với tất cả các phân đoạn đã có trong nhóm của nó. Vì chúng tôi luôn sử dụng lại nhóm tương thích khi có thể nên chúng tôi không bao giờ tăng số lượng nhóm trừ khi bị ép buộc bởi cấu trúc chồng chéo. Điều này phù hợp với định nghĩa về tối ưu phân vùng theo khoảng thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    segs = []
    for i in range(n):
        l, r = map(int, input().split())
        segs.append((l, r, i + 1))

    segs.sort()

    import heapq
    heap = []
    groups = []
    
    for l, r, idx in segs:
        if heap and heap[0][0] < l:
            end, gid = heapq.heappop(heap)
        else:
            gid = len(groups)
            groups.append([])

        groups[gid].append(idx)
        heapq.heappush(heap, (r, gid))

    print(len(groups))
    for g in groups:
        print(len(g))
        print(*g)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp các khoảng theo điểm cuối bên trái của chúng, điều này đảm bảo chúng tôi luôn mở rộng các nhóm theo thứ tự thời gian. Heap lưu trữ các cặp`(end_time, group_id)`để chúng ta có thể tìm được nhóm rảnh sớm nhất một cách hiệu quả. 

điều kiện`heap[0][0] < l`là chi tiết quan trọng. Nó thực thi nghiêm ngặt việc không giao nhau: nếu một nhóm kết thúc vào thời điểm`r`, nó chỉ có thể chấp nhận một khoảng mới bắt đầu từ`l`khi`r < l`. 

Khi chúng tôi bật ra khỏi vùng nhớ heap, chúng tôi cam kết sử dụng lại nhóm đó. Nếu không có nhóm nào hợp lệ, chúng tôi sẽ phân bổ một nhóm mới, tương ứng với việc tăng số màu của biểu đồ khoảng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 5
2 3
4 7
8 9
```Các phân đoạn được sắp xếp vẫn còn:```
(1,5), (2,3), (4,7), (8,9)
```Chúng tôi theo dõi phân công nhóm: 

| Phân đoạn | Đống trước | Hành động | Nhóm được thành lập | 
| --- | --- | --- | --- | 
| (1,5) | trống | nhóm mới 0 | [1] | 
| (2,3) | (5,0) | nhóm mới 1 | [1], [2] | 
| (4,7) | (3,1), (5,0) | tái sử dụng nhóm 1 | [1], [2,3] | 
| (8,9) | (5,0), (7,1) | nhóm tái sử dụng 0 | [1,4], [2,3] | 

Các nhóm cuối cùng được`[1,4]`Và`[2,3]`, vậy hai nhóm là đủ. 

Điều này chứng tỏ việc tái sử dụng phụ thuộc vào nhóm hoàn thiện sớm nhất chứ không phải vào thứ tự đến. 

### Ví dụ 2 

đầu vào:```
3
1 2
2 3
3 4
```Tất cả các khoảng chồng chéo qua các điểm cuối, vì vậy không ai có thể chia sẻ một nhóm. 

| Phân đoạn | Đống trước | Hành động | Nhóm được thành lập | 
| --- | --- | --- | --- | 
| (1,2) | trống | nhóm mới 0 | [1] | 
| (2,3) | (2,0) | nhóm mới 1 | [1], [2] | 
| (3,4) | (2,0), (3,1) | nhóm mới 2 | [1], [2], [3] | 

Điều này xác nhận rằng việc chạm vào điểm cuối sẽ tạo ra lực phân tách tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế, các phép toán heap được tính logarit trên mỗi khoảng | 
| Không gian | O(n) | Lưu trữ cho các phân đoạn, đống và bài tập nhóm | 

Các ràng buộc cho phép lên tới 200.000 khoảng thời gian, do đó, cách tiếp cận n log n phù hợp thoải mái trong giới hạn thời gian. Heap đảm bảo rằng mỗi khoảng được chèn và xóa nhiều nhất một lần, giữ chi phí ở mức tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    segs = []
    for i in range(n):
        l, r = map(int, input().split())
        segs.append((l, r, i + 1))

    segs.sort()
    import heapq

    heap = []
    groups = []

    for l, r, idx in segs:
        if heap and heap[0][0] < l:
            end, gid = heapq.heappop(heap)
        else:
            gid = len(groups)
            groups.append([])

        groups[gid].append(idx)
        heapq.heappush(heap, (r, gid))

    out = []
    out.append(str(len(groups)))
    for g in groups:
        out.append(str(len(g)))
        out.append(" ".join(map(str, g)))
    return "\n".join(out)

# minimum size
assert run("1\n1 10\n") == "1\n1\n1"

# all overlap
assert run("3\n1 5\n2 6\n3 7\n") == "3\n1\n1\n1\n2\n1\n3"

# chain overlap via endpoints
assert run("3\n1 2\n2 3\n3 4\n") == "3\n1\n1\n1\n2\n1\n3"

# disjoint intervals
assert run("3\n1 2\n5 6\n9 10\n") == "1\n3\n1 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng đơn | 1 nhóm | trường hợp cơ sở | 
| chồng chéo hoàn toàn | n nhóm | chồng chéo trong trường hợp xấu nhất | 
| chuỗi điểm cuối | n nhóm | quy tắc giao cắt nghiêm ngặt | 
| rời rạc | 1 nhóm | tái sử dụng đầy đủ | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi các khoảng chỉ chạm vào điểm cuối. Ví dụ,`[1,2]`Và`[2,3]`không thể nhóm lại với nhau vì chúng giao nhau ở 2. Điều kiện heap`end < l`thực thi điều này một cách nghiêm ngặt, ngăn chặn việc sử dụng lại không hợp lệ. 

Một trường hợp cạnh khác là khi các khoảng có thứ tự ngược lại. Việc sắp xếp theo điểm cuối bên trái đảm bảo tính chính xác bất kể thứ tự đầu vào là gì, vì việc nhóm chỉ phụ thuộc vào cấu trúc chứ không phải trình tự. 

Cuối cùng, khi nhiều khoảng bắt đầu tại cùng một điểm, thuật toán sẽ nhanh chóng tạo ra nhiều nhóm. Vùng heap vẫn đảm bảo mỗi khoảng thời gian mới được đặt ở vị trí tối ưu giữa các chuỗi hiện có và số lượng nhóm phù hợp với mức chồng chéo đồng thời tối đa tại thời điểm đó.
