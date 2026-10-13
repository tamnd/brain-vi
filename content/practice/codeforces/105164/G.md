---
title: "CF 105164G - Tháp Đá Granitus"
description: "Chúng ta được tặng một bộ sưu tập các tháp đá, mỗi tháp có chiều cao dương. Hai người chơi luân phiên thực hiện cùng một kiểu di chuyển."
date: "2026-06-27T10:46:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105164
codeforces_index: "G"
codeforces_contest_name: "2024 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 105164
solve_time_s: 101
verified: false
draft: false
---

[CF 105164G - Tháp đá Granitus](https://codeforces.com/problemset/problem/105164/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một bộ sưu tập các tháp đá, mỗi tháp có chiều cao dương. Hai người chơi luân phiên thực hiện cùng một kiểu di chuyển. Trong một lượt, người chơi chọn một số nguyên X lớn hơn 0 và trừ X khỏi mỗi tháp vẫn còn đá, nhưng X không được vượt quá chiều cao của tháp không trống ngắn nhất hiện tại. Các tháp đạt đến 0 sẽ bị loại khỏi trò chơi tiếp theo. 

Do đó, một bước đi được xác định hoàn toàn bằng việc lựa chọn số lớp được bóc ra khỏi đáy của tất cả các tòa tháp còn lại. Mỗi lần di chuyển sẽ làm giảm đồng đều tất cả các tháp đang hoạt động cho đến khi ít nhất một trong số chúng biến mất. 

Người chơi không thể thực hiện một nước đi hợp lệ sẽ thua, điều này xảy ra chính xác khi tất cả các tháp đã giảm về 0 trước lượt của họ hoặc khi không thể có X dương. 

Các ràng buộc cho phép lên tới một triệu tòa tháp, mỗi tòa tháp có chiều cao lên tới một triệu. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào liên tục trừ đi từng lớp, vì trong trường hợp xấu nhất sẽ liên quan đến tối đa 10^12 thao tác nguyên thủy. 

Tình huống cạnh then chốt xuất hiện khi tất cả các tháp đều bằng nhau. Ví dụ: nếu tất cả các độ cao đều giống nhau, mỗi lần di chuyển sẽ xóa hoàn toàn tất cả các tháp còn lại trong một bước. Một trường hợp tinh tế khác là khi chỉ còn lại một tháp, vì khi đó trò chơi giảm xuống việc trừ liên tục một số nguyên dương từ một cọc duy nhất, hoạt động khác với tương tác nhiều tháp nhưng thực sự nhất quán với cùng một cấu trúc. 

Một sai lầm ngây thơ là cho rằng câu trả lời phụ thuộc vào tổng hoặc độ cao tối đa. Ví dụ: với các tháp [1, 100, 100], có thể có cảm giác như các tháp lớn chiếm ưu thế, nhưng tháp nhỏ nhất buộc nước đi đầu tiên là X = 1, sau đó cấu trúc sụp đổ theo cách có thể dự đoán được, điều này chỉ phụ thuộc vào số lượng “cấp” riêng biệt tồn tại. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp coi mỗi lần di chuyển là trừ đi chiều cao tối thiểu hiện tại của tất cả các tháp và loại bỏ những tháp đã cạn kiệt. Điều này đúng vì bất kỳ nước đi tối ưu nào cũng phải luôn chọn đầy đủ X cho phép, nếu không bạn sẽ để đối thủ ở một vị trí tốt hơn với cùng cấu trúc tháp nhưng mức tối thiểu còn lại lớn hơn. Tuy nhiên, ngay cả khi chúng ta luôn trừ đi giá trị tối thiểu, quá trình này vẫn có thể mất tới O(max(ai)) số lần lặp, tốc độ này quá chậm khi chiều cao lớn. 

Quan sát quan trọng là mỗi lần di chuyển sẽ loại bỏ hoàn toàn ít nhất một tháp, vì X được chọn bằng chiều cao tối thiểu hiện tại, do đó ít nhất một tháp trở thành số 0. Sau khi sắp xếp, điều này tương đương với việc bóc mảng theo mức độ cao giảm dần. Mỗi giá trị độ cao riêng biệt đóng góp chính xác một “sự kiện loại bỏ lớp” và trò chơi kết thúc sau khi xử lý tất cả các độ cao riêng biệt. 

Do đó, độ dài của trò chơi chỉ phụ thuộc vào số lượng độ cao riêng biệt tồn tại chứ không phụ thuộc vào độ lớn hoặc số lượng của chúng. Mỗi giá trị riêng biệt tương ứng với một bước di chuyển bắt buộc trong đó mức tối thiểu tăng sau khi loại bỏ tất cả các tháp bằng mức tối thiểu đó. 

Người chiến thắng được xác định hoàn toàn bằng số lần giảm như vậy. Nếu số mức độ cao khác nhau là số lẻ thì người chơi đầu tiên thực hiện nước đi cuối cùng và giành chiến thắng. Nếu chẵn thì người chơi thứ hai sẽ thắng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng loại bỏ lớp | O(N * max(ai)) | O(N) | Quá chậm | 
| Đếm chiều cao khác biệt | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Sắp xếp danh sách các chiều cao của tháp theo thứ tự không giảm sao cho các chiều cao giống hệt nhau liền kề nhau. Điều này cho phép chúng tôi phát hiện thời điểm thay đổi tối thiểu sau khi xóa một lớp. 
2. Duyệt qua danh sách đã sắp xếp và đếm số lần giá trị thay đổi so với danh sách trước đó. Mỗi thay đổi tương ứng với một mức độ cao riêng biệt mới xuất hiện. 
3. Coi số lượng các giá trị riêng biệt là số nước đi trong trò chơi. Điều này hợp lệ vì mỗi chiều cao riêng biệt sẽ bị loại bỏ trong một thao tác bắt buộc khi nó trở thành mức tối thiểu. 
4. Nếu số chiều cao khác nhau là số lẻ thì tuyên bố Alicius là người chiến thắng. Ngược lại, Bobius sẽ thắng. 

Lý do chính là sau mỗi lần di chuyển, ít nhất toàn bộ một cấp độ cao sẽ biến mất và không có cấp độ cao nào có thể xuất hiện lại hoặc bị chia cắt một phần theo cách làm thay đổi cấu trúc trong tương lai. Do đó, trạng thái trò chơi sụp đổ một cách đơn điệu thông qua các giá trị riêng biệt được sắp xếp. 

### Tại sao nó hoạt động 

Điều bất biến là sau k lần di chuyển, chính xác k mức độ cao riêng biệt đã bị loại bỏ hoàn toàn và các tòa tháp còn lại tương ứng chính xác với hậu tố của danh sách độ cao duy nhất được sắp xếp. Không có điểm quyết định nào cho phép bỏ qua một cấp độ hoặc hợp nhất các cấp độ, vì X buộc phải bằng mức tối thiểu hiện tại. Điều này làm cho chuỗi các bước di chuyển mang tính xác định theo thứ tự của các phần tử bằng nhau, do đó toàn bộ trò chơi giảm xuống việc đếm xem có bao nhiêu điểm cực tiểu khác biệt tồn tại theo thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n_and_rest = input().split()
    if not n_and_rest:
        return
    n = int(n_and_rest[0])
    arr = list(map(int, n_and_rest[1:]))

    # if input didn't fully come in first line
    while len(arr) < n:
        arr.extend(map(int, input().split()))

    arr.sort()

    moves = 0
    prev = None

    for x in arr:
        if x != prev:
            moves += 1
            prev = x

    if moves % 2 == 1:
        print("Alicius")
    else:
        print("Bobius")

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên đảm bảo khả năng phân tích cú pháp đầu vào mạnh mẽ vì các giá trị có thể trải dài trên nhiều dòng. Việc sắp xếp tập hợp các độ cao bằng nhau để có thể phát hiện các chuyển tiếp trong một lần quét. Biến`moves`đếm các nhóm chiều cao riêng biệt, tương ứng trực tiếp với số lần buộc phải loại bỏ. 

Một điểm tinh tế là xử lý đầu vào trong đó dòng đầu tiên có thể chứa cả hai`N`và một phần của mảng, đó là lý do tại sao trình phân tích cú pháp tích lũy các giá trị cho đến khi đạt đến N. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[4, 5, 3, 3, 10]`Đã sắp xếp:`[3, 3, 4, 5, 10]`| Giá trị | Trước | Riêng biệt? | Di chuyển | 
| --- | --- | --- | --- | 
| 3 | - | vâng | 1 | 
| 3 | 3 | không | 1 | 
| 4 | 3 | vâng | 2 | 
| 5 | 4 | vâng | 3 | 
| 10 | 5 | vâng | 4 | 

Nước đi = 4, chẵn, do đó Bobius thắng. 

Điều này xác nhận rằng các giá trị lặp lại không làm tăng số lần di chuyển, chỉ thay đổi về chiều cao mới quan trọng. 

### Ví dụ 2 

đầu vào:`[1, 7, 8, 10, 15]`Đã sắp xếp rồi. 

| Giá trị | Trước | Riêng biệt? | Di chuyển | 
| --- | --- | --- | --- | 
| 1 | - | vâng | 1 | 
| 7 | 1 | vâng | 2 | 
| 8 | 7 | vâng | 3 | 
| 10 | 8 | vâng | 4 | 
| 15 | 10 | vâng | 5 | 

Nước đi = 5, lẻ nên Alicius thắng. 

Điều này cho thấy trường hợp cực đoan trong đó tất cả các giá trị đều khác biệt, khiến mỗi bước trở thành một sự kiện rút gọn bắt buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | sắp xếp thống trị truyền tải | 
| Không gian | O(N) | lưu trữ mảng | 

Giải pháp này phù hợp thoải mái trong giới hạn vì việc sắp xếp một triệu số nguyên là khả thi trong Python và lần quét tiếp theo là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples (interpreted)
assert run("3\n4 5 3 3 10\n") == "Bobius"
assert run("5\n1 7 8 10 15\n") == "Alicius"

# minimum size
assert run("1\n1\n") == "Alicius"

# all equal
assert run("4\n5 5 5 5\n") == "Alicius"

# strictly increasing
assert run("4\n1 2 3 4\n") == "Bobius"

# mixed duplicates
assert run("6\n2 2 2 3 3 4\n") == "Bobius"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 tháp đơn | Alicius | trường hợp cơ sở | 
| tất cả các giá trị bằng nhau | Alicius | di chuyển duy nhất sụp đổ | 
| trình tự tăng dần | Bobius | chuyển tiếp khác biệt tối đa | 
| trùng lặp trộn lẫn | Bobius | đếm khác biệt chính xác | 

## Vỏ cạnh 

Đối với một tòa tháp duy nhất như`[7]`, việc sắp xếp sẽ cho một phần tử, do đó có chính xác một giá trị riêng biệt. Thuật toán tính một nước đi là số lẻ nên Alicius thắng. Điều này phù hợp với thực tế là người chơi đầu tiên có thể chọn X = 7 và kết thúc trò chơi ngay lập tức. 

Đối với tất cả các tòa tháp bằng nhau như`[5, 5, 5]`, việc sắp xếp vẫn tạo ra một giá trị riêng biệt. Chỉ một lần di chuyển xảy ra: trừ 5 một lần và tất cả các tháp đều biến mất. Thuật toán đưa ra Alicius, phù hợp với lối chơi tối ưu vì nước đi đầu tiên cũng là nước đi cuối cùng. 

Đối với các bản sao xen kẽ như`[1, 1, 2, 2, 3, 3]`, số đếm riêng biệt là ba. Người chơi đầu tiên thực hiện hai lần giảm đầu tiên một cách gián tiếp thông qua cấu trúc và vì ba là số lẻ nên Alicius thắng. Điều này cho thấy rằng tính đa dạng không ảnh hưởng đến thời lượng chơi mà chỉ có các mức độ cao khác nhau mới ảnh hưởng.
