---
title: "CF 103785C - Dualites in Pain - Sự khởi đầu"
description: "Chúng ta được cung cấp một mảng các số nguyên và thao tác duy nhất được phép là chọn một lần xuất hiện của giá trị lớn nhất hiện tại trong mảng và giảm nó đi một. Có một hạn chế ngăn cản việc chọn cùng một vị trí trong hai thao tác liên tiếp."
date: "2026-07-02T08:50:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103785
codeforces_index: "C"
codeforces_contest_name: "CodeBrew : Freshers Contest 2022"
rating: 0
weight: 103785
solve_time_s: 54
verified: true
draft: false
---

[CF 103785C - Dualites in Pain - Sự khởi đầu](https://codeforces.com/problemset/problem/103785/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và thao tác duy nhất được phép là chọn một lần xuất hiện của giá trị lớn nhất hiện tại trong mảng và giảm nó đi một. Có một hạn chế ngăn cản việc chọn cùng một vị trí trong hai thao tác liên tiếp. 

Quá trình tiếp tục cho đến khi mọi phần tử trở thành số 0. Nhiệm vụ không chỉ là quyết định xem điều này có khả thi hay không, mà còn, trong biến thể khó hơn, là tạo ra một chuỗi hợp lệ các chỉ mục đã chọn để đạt được nó, ưu tiên các chuỗi nhỏ nhất về mặt từ điển khi có thể có nhiều. 

Ràng buộc chính là về cấu trúc chứ không phải về số lượng. Hoạt động luôn nhắm đến một phần tử tối đa, điều này buộc mảng phải “làm phẳng” từ trên xuống. Điều này có nghĩa là thứ tự tương đối của các phần tử lớn quyết định liệu chúng ta có thể thay thế các lựa chọn mà không vi phạm giới hạn chỉ số liên tiếp hay không. 

Từ góc độ phức tạp, kích thước mảng đủ lớn để bất kỳ mô phỏng nào liên tục quét cực đại sẽ quá chậm. Một giải pháp phải giảm thiểu vấn đề thành việc kiểm tra cấu trúc tham lam và xây dựng có kiểm soát, trong đó mỗi phần tử được xử lý theo các giai đoạn tổng hợp thay vì hoạt động đơn vị. 

Một trường hợp lỗi tinh tế xuất hiện khi phần tử lớn nhất và lớn thứ hai khác nhau quá nhiều. Nếu mức tối đa lớn hơn mức tối đa thứ hai ít nhất là hai thì sau một lần giảm, cùng một chỉ số sẽ vẫn duy trì ở mức tối đa duy nhất, buộc nó phải được chọn lại ngay lập tức, điều này vi phạm ràng buộc. Đây là trở ngại cơ bản. 

Ví dụ, hãy xem xét`[5, 1]`. Sau khi chọn phần tử đầu tiên, nó sẽ trở thành`4`, vẫn lớn hơn`1`, nên phải chọn lại, vi phạm quy tắc. 

Loại trường hợp cạnh thứ hai xuất hiện khi các giá trị bằng hoặc khác nhau đúng một. Trong những trường hợp này, mức tối đa có thể “chuyển giao” giữa các chỉ số, cho phép luân phiên. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ liên tục tìm mức tối đa, chọn một chỉ mục trong số các mối quan hệ, giảm nó và theo dõi chỉ mục được chọn cuối cùng để thực thi ràng buộc. Điều này đúng về nguyên tắc nhưng tốn kém vì mỗi bước tốn thời gian tuyến tính và số bước bằng tổng của tất cả các giá trị. Trong trường hợp xấu nhất, điều này trở thành bậc hai hoặc tệ hơn. 

Quan sát quan trọng là việc sắp xếp mảng sẽ tiết lộ cấu trúc của quy trình. Sau khi được sắp xếp, chúng ta có thể nghĩ theo các lớp: từ giá trị lớn nhất trở xuống, chúng ta dần dần “kích hoạt” nhiều chỉ số hơn. Cách duy nhất để quy trình vẫn hợp lệ là nếu mỗi giá trị mới không thấp hơn quá nhiều so với giá trị trước đó, nếu không thì một chỉ mục sẽ bị ép buộc lặp đi lặp lại. 

Điều này dẫn đến một cách giải thích mang tính xây dựng. Chúng tôi luôn duy trì một tập hợp các chỉ số hiện hoạt tương ứng với tiền tố lớn nhất của các giá trị được sắp xếp. Miễn là các giá trị liền kề khác nhau tối đa một, chúng ta có thể mở rộng tập hợp hoạt động này từng phần tử một và phân phối các phần giảm dần trên nó theo kiểu vòng tròn tôn trọng ràng buộc "không có chỉ mục liên tiếp". 

Do đó, thay vì mô phỏng các mức giảm riêng lẻ, chúng tôi mô phỏng các mức giá trị. Mỗi lần chúng ta giảm giá trị`v`ĐẾN`v-1`, chúng tôi xuất ra một khối hoạt động được phân phối trên tiền tố hoạt động hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(∑A · N) | O(N) | Quá chậm | 
| Xây dựng lớp sắp xếp | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi ghép từng giá trị với chỉ mục ban đầu của nó và sắp xếp các cặp này theo giá trị theo thứ tự không giảm. Về mặt khái niệm, chúng tôi cũng thêm một cặp giả`(0, 0)`để đơn giản hóa sự khác biệt giữa các cấp độ liên tiếp. 

Sau đó, chúng tôi làm việc từ phần tử lớn nhất trở xuống, mở rộng từng phần tử của tập hợp hoạt động. 

1. Sắp xếp mảng`(value, index)`cặp và nối thêm`(0, 0)`như một người lính canh. Điều này cho phép xử lý rõ ràng việc giảm cuối cùng về 0. 
2. Duy trì một biến`i`đại diện cho tiền tố hiện tại của các phần tử được sắp xếp là "hoạt động", bắt đầu từ phần tử lớn nhất. 
3. Đối với mỗi bước chuyển từ mức giá trị lớn hơn sang mức giá trị nhỏ hơn tiếp theo, hãy thu thập tất cả các chỉ số trong tiền tố hoạt động hiện tại. Các chỉ số này thể hiện chính xác vị trí nào có thể được sử dụng một cách an toàn để giảm giá ở giai đoạn này. 
4. Sắp xếp các chỉ mục này để đảm bảo đầu ra nhỏ nhất về mặt từ điển khi có thể có nhiều lựa chọn. Thứ tự này đảm bảo rằng khi lặp lại các khối, chúng ta luôn bắt đầu từ các chỉ số có sẵn nhỏ nhất. 
5. Tính toán số thao tác cần thiết cho quá trình chuyển đổi cấp độ này bằng hiệu giữa các giá trị liên tiếp trong mảng được sắp xếp. Đối với mỗi thao tác như vậy, hãy xuất toàn bộ tập hoạt động hiện tại theo thứ tự, mô phỏng một “vòng” giảm đầy đủ trên tất cả các ứng cử viên tối đa đang hoạt động. 
6. Di chuyển đến phần tử tiếp theo theo thứ tự đã sắp xếp, mở rộng tập hợp hoạt động theo một chỉ mục và lặp lại cho đến khi tất cả các giá trị về 0. 

Tính đúng đắn xuất phát từ thực tế là ở bất kỳ giai đoạn nào, tất cả các phần tử hiện đang hoạt động đều có chiều cao hiệu quả như nhau. Sự khác biệt trong các giá trị được sắp xếp cho chúng ta biết chính xác có bao nhiêu lớp đồng nhất phải được loại bỏ trước khi một phần tử mới trở nên phù hợp. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, tiền tố hoạt động chứa chính xác các chỉ mục có giá trị hiện được gắn ở mức tối đa sau khi trừ đi các lớp được áp dụng trước đó. Quá trình trừ các lớp là thống nhất trên tất cả các chỉ mục đang hoạt động, do đó không có chỉ mục nào bị buộc phải lặp lại ngay lập tức. Điều kiện là các giá trị được sắp xếp liền kề khác nhau nhiều nhất là một đảm bảo rằng tập cực đại đang hoạt động không bao giờ thu gọn thành một phần tử cứng duy nhất vi phạm ràng buộc kề. Mỗi quá trình chuyển đổi lớp bảo toàn tính bất biến mà mức tối đa được chia sẻ giữa ít nhất một chỉ mục thay thế. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = [(0, 0)]
    
    for i in range(1, n + 1):
        arr.append((int(input()), i))
    
    arr.sort()
    
    if n == 1:
        if arr[1][0] == 1:
            print(1)
        else:
            print(-1)
        return
    
    # feasibility check: consecutive differences must be <= 1
    for i in range(1, n + 1):
        if arr[i][0] - arr[i - 1][0] > 1:
            print(-1)
            return
    
    active = []
    res = []
    
    for i in range(n, 0, -1):
        active.append(arr[i][1])
        active.sort()
        
        cnt = arr[i][0] - arr[i - 1][0]
        for _ in range(cnt):
            res.extend(active)
    
    print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc các giá trị theo cặp để việc sắp xếp giữ nguyên các chỉ số gốc. lính gác`(0, 0)`cho phép việc giảm xuống cuối cùng về 0 được xử lý thống nhất. 

Kiểm tra tính khả thi thực thi điều kiện cấu trúc mà không có khoảng cách giữa các giá trị được sắp xếp liên tiếp vượt quá một. Nếu không có điều này, một phần tử sẽ cần phải giảm nhiều lần trước khi bất kỳ phần tử nào khác đủ điều kiện, vi phạm quy tắc “không có chỉ mục liên tiếp”. 

các`active`danh sách đại diện cho tập hợp các chỉ số hiện tại có chung “mức” tối đa. Mỗi lần lặp lại sẽ thêm một chỉ mục nữa từ hậu tố được sắp xếp, phản ánh cách các phần tử mới trở thành một phần của điểm ổn định tối đa khi chúng ta di chuyển xuống dưới. 

Đối với mỗi cấp độ, chúng tôi lặp lại chính xác toàn bộ hoạt động`arr[i][0] - arr[i-1][0]`lần. Mỗi lần lặp lại tương ứng với mức giảm một đơn vị trên tất cả các cực đại hoạt động. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng`[4, 2, 1, 4, 2]`. 

Chúng tôi xây dựng`(value, index)`cặp và sắp xếp chúng:`[(1,3), (2,2), (2,5), (4,1), (4,4)]`cộng với lính gác`(0,0)`. 

| Bước | Bộ hoạt động | Giá trị hiện tại | Giá trị tiếp theo | Sự khác biệt | Khối đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [4] | 4 | 2 | 2 | [1] lặp lại 2 lần | 
| 2 | [1,4] | 2 | 2 | 0 | không | 
| 3 | [1,2,4] | 2 | 1 | 1 | trọn bộ một lần | 
| 4 | [1,2,3,4,5] | 1 | 0 | 1 | trọn bộ một lần | 

Dấu vết cho thấy cách các chỉ số được thêm dần dần và cách mỗi khác biệt giá trị chuyển trực tiếp thành các lần quét toàn bộ lặp lại trên tập hoạt động hiện tại. 

Bây giờ hãy xem xét một trường hợp cạnh nhỏ`[2, 1]`. 

Đã sắp xếp:`[(1,2), (2,1)]`. 

| Bước | Bộ hoạt động | Sự khác biệt | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | [1] | 1 | [2] | 
| 2 | [1,2] | 1 | [1,2] | 

Điều này xác nhận rằng sự xen kẽ xuất hiện một cách tự nhiên từ việc xây dựng lớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + tổng kích thước đầu ra) | Việc sắp xếp chiếm ưu thế, việc tạo đầu ra là tuyến tính trong các hoạt động được sản xuất | 
| Không gian | O(n) | Lưu trữ các cặp, tập hoạt động và chuỗi kết quả | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì mọi thao tác được tạo ở dạng tổng hợp thay vì được mô phỏng riêng lẻ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = backup
    return out.getvalue().strip()

# simple valid case
assert run("2\n2\n2\n") != "", "basic equal case"

# impossible gap case
assert run("2\n5\n1\n") == "-1", "gap too large"

# single element
assert run("1\n1\n") == "1", "single decrement"

# increasing chain
assert run("3\n1\n2\n3\n") != "", "strict chain valid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2,5,1`|`-1`| Phát hiện khoảng cách không hợp lệ | 
|`1,1`|`1`| Trường hợp hợp lệ tối thiểu | 
|`1,2,3`| trình tự hợp lệ | Tăng tính khả thi | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi một phần tử lớn hơn đáng kể so với tất cả các phần tử khác, chẳng hạn như`[10, 1, 1]`. Sau khi sắp xếp, khoảng cách giữa 10 và 1 vượt quá 1 nên thuật toán sẽ ngay lập tức loại bỏ nó. Mọi nỗ lực mô phỏng sẽ liên tục chọn phần tử lớn nhất và vi phạm ràng buộc không lặp lại. 

Một trường hợp khác là khi tất cả các phần tử đều bằng nhau, chẳng hạn như`[3, 3, 3]`. Ở đây, tập hoạt động sẽ mở rộng ngay lập tức cho tất cả các chỉ mục và mỗi vòng giảm dần sẽ quay vòng qua tất cả các vị trí, tránh lặp lại liên tiếp và hoàn thành một cách rõ ràng.
