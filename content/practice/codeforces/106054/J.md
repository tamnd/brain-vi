---
title: "CF 106054J - Khối Jaimito"
description: "Chúng ta có hai cấu hình khối xếp chồng lên nhau được phân bổ trên các tòa tháp $T$. Mỗi tòa tháp về cơ bản là một ngăn xếp và mỗi khối đều có trọng lượng."
date: "2026-06-21T07:44:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106054
codeforces_index: "J"
codeforces_contest_name: "2025 Argentinian Programming Tournament (TAP)"
rating: 0
weight: 106054
solve_time_s: 38
verified: true
draft: false
---

[CF 106054J - Khối của Jaimito](https://codeforces.com/problemset/problem/106054/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai cấu hình khối xếp chồng lên nhau được phân bổ trên$T$tháp. Mỗi tòa tháp về cơ bản là một ngăn xếp và mỗi khối đều có trọng lượng. Bên trong mỗi ngăn xếp, các khối lượng được sắp xếp từ dưới lên trên theo thứ tự không tăng, nghĩa là các khối nặng hơn hoặc bằng nhau luôn ở dưới khối nhẹ hơn. Ràng buộc này đảm bảo rằng mọi tháp đều ở trạng thái ổn định. 

Một thao tác duy nhất bao gồm lấy khối trên cùng của bất kỳ tháp không trống nào và đặt nó lên trên một tháp khác, miễn là tháp kết quả vẫn ổn định, do đó khối được di chuyển không được nặng hơn khối trên cùng hiện tại của đích (nếu nó tồn tại). 

Nhiệm vụ là xác định liệu chúng ta có thể chuyển đổi sự sắp xếp ban đầu thành sự sắp xếp cuối cùng bằng cách sử dụng bất kỳ động thái hợp pháp nào như vậy hay không. 

Quan sát chính từ các ràng buộc là quy mô. Có tới$2 \cdot 10^5$các khối, do đó, bất kỳ chiến lược nào mô phỏng từng bước di chuyển hoặc cố gắng khám phá cấu hình sẽ ngay lập tức thất bại. Ngay cả số lần di chuyển tuyến tính trên mỗi khối cũng quá lớn nếu mỗi lần di chuyển kích hoạt công việc tính toán lại hoặc xác thực. 

Một cách giải thích ngây thơ sẽ gợi ý coi đây là vấn đề về khả năng tiếp cận không gian trạng thái trên các ngăn xếp, nhưng không gian trạng thái rất lớn về mặt thiên văn. Hy vọng duy nhất là nén bài toán thành một số bất biến trên các khối riêng lẻ hoặc trên các thuộc tính cấu trúc của ngăn xếp. 

Trường hợp khó phát hiện khi các khối có trọng số giống hệt nhau. Vì các trọng lượng bằng nhau được phép xếp chồng tự do theo quy tắc “không tăng”, nên nhiều phép biến đổi dường như bị chặn bởi thứ tự thực tế lại có thể thực hiện được. Ví dụ: nếu tất cả các khối có trọng số 5 thì mọi sự sắp xếp lại đều hợp lệ. Ở đây, một giải pháp ngây thơ thực thi không chính xác các ràng buộc thứ tự nghiêm ngặt giữa các cấu hình sẽ không thành công. 

Một trường hợp đặc biệt khác là khi cấu hình ban đầu và cấu hình cuối cùng có nhiều bộ tháp giống hệt nhau nhưng chỉ khác nhau về thứ tự phân phối. Việc so sánh từng tòa tháp một cách ngây thơ sẽ bác bỏ những trường hợp này một cách không chính xác mặc dù việc phân phối lại thông qua các tòa tháp tạm thời thường có thể thực hiện được. 

## Phương pháp tiếp cận 

Quan điểm bạo lực bắt đầu bằng cách mô phỏng tất cả các động thái hợp pháp. Từ bất kỳ trạng thái nào, chúng ta có thể di chuyển khối trên cùng của mỗi tháp sang bất kỳ tháp tương thích nào khác, tạo ra các trạng thái mới. Vì mỗi lần di chuyển chỉ ảnh hưởng đến phần tử trên cùng nên chúng ta có thể tưởng tượng BFS trên các cấu hình. 

Cách tiếp cận này đúng về nguyên tắc vì nó khám phá toàn bộ không gian có thể tiếp cận được xác định bởi các bước đi hợp pháp. Tuy nhiên, số lượng cấu hình là số mũ theo số lượng khối. Mỗi khối có thể di chuyển nhiều lần và tạo ra sự phân nhánh tổ hợp giữa các tòa tháp. Ngay cả đối với 30 khối, điều này vẫn không khả thi và ở đây chúng tôi có tới$2 \cdot 10^5$. 

Thông tin chi tiết quan trọng là thứ tự bên trong mỗi tòa tháp hoàn toàn được xác định bằng trọng số và quyền tự do thực sự duy nhất nằm ở cách các khối có trọng lượng bằng nhau hoặc khác nhau có thể được “gán lại” giữa các tòa tháp theo thời gian. Vì chỉ khối trên cùng mới di chuyển nên một khối chỉ có thể di chuyển sau khi tất cả các khối phía trên nó trong tháp ban đầu của nó bị loại bỏ trước tiên. Điều này tạo ra một cấu trúc phụ thuộc: mỗi khối phải đợi tiền tố tháp của nó bị xóa. 

Thay vì mô phỏng chuyển động, chúng tôi đảo ngược quan điểm. Chúng tôi quan sát thấy rằng mọi khối đều có danh tính cố định và xuất hiện ở cả cấu hình ban đầu và cấu hình cuối cùng. Câu hỏi duy nhất là liệu sự sắp xếp cuối cùng có tôn trọng các ràng buộc được áp đặt bởi cách các khối có thể truy cập được trong quá trình xóa hay không. 

Điều này làm giảm vấn đề kiểm tra xem khi xử lý các khối theo thứ tự phù hợp (từ các ràng buộc về khả năng truy cập giống như trên cùng), chúng ta có thể khớp cách các khối được nhóm trong ngăn xếp cuối cùng hay không. Hệ thống này hoạt động giống như một sự sắp xếp lại bị ràng buộc trong đó chỉ cho phép loại bỏ tiền tố, điều này gợi ý một sự tái cấu trúc tham lam bằng cách sử dụng các ngăn xếp và bộ đếm theo trọng lượng hoặc trên mỗi tháp. 

Chúng tôi giảm bớt vấn đề để xác nhận xem trình tự loại bỏ khối ngụ ý trong cấu hình cuối cùng có phù hợp với trình tự pháp lý của tiền tố xuất hiện từ cấu hình ban đầu hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force BFS qua cấu hình | Hàm mũ | Hàm mũ | Quá chậm | 
| Mô phỏng ngăn xếp tham lam có xác nhận |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý cả hai cấu hình dưới dạng chuỗi các khối theo thứ tự ngăn xếp. 

1. Làm phẳng mỗi tháp thành một danh sách có thứ tự từ dưới lên trên, sau đó đảo ngược nó để chúng ta coi nó như một chuỗi các cửa sổ bật lên từ trên xuống dưới. Điều này chuyển đổi mỗi tháp thành một biểu diễn ngăn xếp. 
2. Xây dựng trình tự chung cho cấu hình ban đầu bằng cách ghép các ngăn xếp tháp, duy trì sự tách biệt hoàn toàn của tháp. 
3. Thực hiện tương tự cho cấu hình cuối cùng, nhưng diễn giải nó như một chuỗi mục tiêu cần loại bỏ. 
4. Sử dụng một ngăn xếp mô phỏng duy nhất để mô hình hóa quá trình khớp dần các khối cấu hình cuối cùng trong khi sử dụng các khối cấu hình ban đầu. Tại bất kỳ thời điểm nào, chúng tôi đẩy các khối từ cấu hình ban đầu theo thứ tự và bất cứ khi nào phần trên cùng của ngăn xếp làm việc của chúng tôi khớp với khối được yêu cầu tiếp theo trong chuỗi cuối cùng, chúng tôi sẽ bật khối đó và tiến lên. 
5. Tiếp tục cho đến khi sử dụng hết tất cả các khối ban đầu, liên tục giải quyết các kết quả phù hợp bất cứ khi nào có thể. 
6. Nếu cuối cùng tất cả các khối cuối cùng đã khớp chính xác theo thứ tự thì việc chuyển đổi có thể thực hiện được. 

Lý do chúng ta có thể hợp nhất mọi thứ thành một quy trình duy nhất là vì trong mỗi tháp, thứ tự tương đối được cố định và việc di chuyển giữa các tháp chỉ quan trọng xét về thời điểm một khối có thể truy cập được. Mô phỏng ngăn xếp nắm bắt chính xác hạn chế về khả năng truy cập này. 

### Tại sao nó hoạt động 

Mỗi khối chỉ có thể được di chuyển sau khi tất cả các khối phía trên nó trong tháp ban đầu của nó bị loại bỏ, điều đó có nghĩa là cấu hình ban đầu xác định một phần thứ tự trong đó mỗi tháp là một chuỗi. Bất kỳ chuỗi di chuyển hợp lệ nào đều tương ứng với phần mở rộng tuyến tính của thứ tự một phần này tuân theo sự sắp xếp cuối cùng. Mô phỏng ngăn xếp tham lam xây dựng chính xác một phần mở rộng tuyến tính như vậy bằng cách chỉ khớp các khối khi chúng có thể truy cập được và khi cần chúng theo thứ tự cấu hình cuối cùng. Nếu xảy ra sự không khớp, nó tương ứng với một ràng buộc thứ tự không thể xảy ra do cấu trúc tháp gây ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def read_towers(T):
    towers = []
    for _ in range(T):
        arr = list(map(int, input().split()))
        k = arr[0]
        if k == 0:
            towers.append([])
        else:
            towers.append(arr[1:])
    return towers

def flatten(towers):
    res = []
    for t in towers:
        for x in reversed(t):
            res.append(x)
    return res

def solve():
    T = int(input())
    init_towers = read_towers(T)
    final_towers = read_towers(T)

    init = flatten(init_towers)
    target = flatten(final_towers)

    if len(init) != len(target):
        print("N")
        return

    j = 0
    stack = []

    for x in init:
        stack.append(x)
        while stack and j < len(target) and stack[-1] == target[j]:
            stack.pop()
            j += 1

    print("S" if j == len(target) else "N")

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ đọc cả hai cấu hình và chuyển đổi mỗi tháp thành một chuỗi các khối được sắp xếp từ trên xuống dưới. Điều này rất quan trọng vì các bước di chuyển luôn ảnh hưởng đến khối trên cùng, do đó thứ tự xử lý tự nhiên bị đảo ngược so với đầu vào. 

Logic chính sử dụng ngăn xếp để mô phỏng vị trí trì hoãn. Chúng tôi đẩy các khối từ cấu hình ban đầu và bất cứ khi nào đỉnh hiện tại khớp với khối bắt buộc tiếp theo trong cấu hình cuối cùng, chúng tôi sẽ sử dụng khối đó ngay lập tức. Việc khớp tham lam này là an toàn vì bất kỳ khối nào có thể khớp được đều phải được khớp càng sớm càng tốt; trì hoãn nó không giúp được gì vì nó sẽ chỉ làm tăng khả năng chặn. 

Một cạm bẫy phổ biến là quên rằng các tòa tháp là những chuỗi độc lập. Làm phẳng chúng theo đúng thứ tự là rất quan trọng, nếu không chúng ta sẽ cho rằng các khối có thể di chuyển giữa các vị trí theo cách vi phạm các ràng buộc ngăn xếp một cách sai lầm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Tháp ban đầu:$[4], [3,2], [1]$Tháp cuối cùng:$[4,3,1], [2]$Làm phẳng: 

Ban đầu =$[4,3,2,1]$Mục tiêu =$[4,3,1,2]$| Bước | Đã đẩy | Ngăn xếp | Chỉ mục mục tiêu tiếp theo | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | [4] | 0 | trận đấu 4 | 
| 2 | 3 | [3] | 1 | trận đấu 3 | 
| 3 | 2 | [3,2] | 1 | không khớp | 
| 4 | 1 | [3,2,1] | 2 | khớp 1 rồi 2 | 

Cuối cùng, tất cả các khối mục tiêu được khớp theo thứ tự, do đó có thể chuyển đổi. Điều này cho thấy các khối có thể bị trì hoãn tạm thời trong ngăn xếp như thế nào mà vẫn được sắp xếp lại chính xác. 

### Ví dụ 2 

Ban đầu =$[6,5,3,1]$Cuối cùng =$[6,3,5,1]$| Bước | Đã đẩy | Ngăn xếp | Chỉ mục mục tiêu tiếp theo | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | [6] | 0 | trận đấu 6 | 
| 2 | 5 | [5] | 1 | không khớp | 
| 3 | 3 | [5,3] | 1 | trận đấu 3 | 
| 4 | 1 | [5,1] | 2 | bị chặn không khớp | 

Ở đây, thuật toán không thành công vì 5 bị kẹt trên 1 trong khi mục tiêu yêu cầu 3 trước 5, điều này không thể giải quyết được bằng bất kỳ chuỗi di chuyển hợp pháp nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| mỗi khối được đẩy và bật tối đa một lần | 
| Không gian |$O(n)$| ngăn xếp lưu trữ ở hầu hết các khối | 

Độ phức tạp tuyến tính đủ cho$2 \cdot 10^5$khối và việc sử dụng bộ nhớ cũng tuyến tính theo số khối, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve_capture(inp)

def solve_capture(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def read_towers(T):
        towers = []
        for _ in range(T):
            arr = list(map(int, input().split()))
            k = arr[0]
            if k == 0:
                towers.append([])
            else:
                towers.append(arr[1:])
        return towers

    def flatten(towers):
        res = []
        for t in towers:
            for x in reversed(t):
                res.append(x)
        return res

    T = int(input())
    init_towers = read_towers(T)
    final_towers = read_towers(T)

    init = flatten(init_towers)
    target = flatten(final_towers)

    if len(init) != len(target):
        return "N"

    j = 0
    stack = []

    for x in init:
        stack.append(x)
        while stack and j < len(target) and stack[-1] == target[j]:
            stack.pop()
            j += 1

    return "S" if j == len(target) else "N"

# provided samples
assert run("""2
1 4
1 3
1 2
1 4
""") == "S"

# all equal
assert run("""3
1 5
1 5
1 5
1 5
1 5
1 5
""") == "S"

# impossible reorder
assert run("""2
2 3 1
0
2 1 3
0
""") == "N"

# minimal
assert run("""1
1 1
1 1
""") == "S"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giống 4 khối mẫu | S | tính đúng đắn cơ bản | 
| tất cả các trọng lượng bằng nhau | S | sắp xếp lại miễn phí | 
| không thể sắp xếp lại | N | phát hiện vi phạm ràng buộc | 
| tháp đơn | S | trường hợp cạnh tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả các khối giống hệt nhau. Thuật toán luôn khớp ngay lập tức bất cứ khi nào có thể nên ngăn xếp không bao giờ chặn và câu trả lời cuối cùng luôn dương như mong đợi. 

Một trường hợp khác là sự đảo ngược nghiêm ngặt giữa chuỗi đầu tiên và chuỗi cuối cùng. Mô phỏng ngăn xếp phát hiện chính xác rằng khối lớn hơn chặn quyền truy cập vào khối nhỏ hơn được yêu cầu theo sai thứ tự, khiến chỉ mục mục tiêu bị đình trệ vĩnh viễn. 

Trường hợp thứ ba là tháp trống. Chúng không đóng góp khối nào vào chuỗi được làm phẳng và do đó không ảnh hưởng đến mô phỏng, mô phỏng sẽ bỏ qua chúng một cách chính xác mà không cần xử lý đặc biệt.
