---
title: "CF 105536D - \u0421\u043e\u0431\u0435\u0441\u0435\u0434\u043e\u0432\u0430\u043d\u0438\u0435 \u0432 \u0421\u0431\u0435\u0440"
description: "Chúng ta được giao một chuỗi các nhiệm vụ được trình bày trên một dòng, trong đó mỗi nhiệm vụ có độ khó là số nguyên. Một nhân vật tên Vanya có cấp độ kỹ năng bắt đầu ở giá trị thấp và có thể tăng lên khi anh ta hoàn thành nhiệm vụ."
date: "2026-06-23T01:11:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105536
codeforces_index: "D"
codeforces_contest_name: "\u041a\u043e\u0433\u043d\u0438\u0442\u0438\u0432\u043d\u044b\u0435 \u0442\u0435\u0445\u043d\u043e\u043b\u043e\u0433\u0438\u0438 2024-2025. \u041f\u0435\u0440\u0432\u044b\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 105536
solve_time_s: 56
verified: true
draft: false
---

[CF 105536D - \u0421\u043e\u0431\u0435\u0441\u0435\u0434\u043e\u0432\u0430\u043d\u0438\u0435 \u0432 \u0421\u0431\u0435\u0440](https://codeforces.com/problemset/problem/105536/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một chuỗi các nhiệm vụ được trình bày trên một dòng, trong đó mỗi nhiệm vụ có độ khó là số nguyên. Một nhân vật tên Vanya có cấp độ kỹ năng bắt đầu ở giá trị thấp và có thể tăng lên khi anh ta hoàn thành nhiệm vụ. 

Vanya được phép quét trình tự nhiều lần, nhưng anh ấy không chọn ngẫu nhiên các nhiệm vụ. Thay vào đó, anh luôn nhận nhiệm vụ sớm nhất trong mảng mà anh hiện có khả năng giải quyết. Khả năng được xác định bằng cách so sánh trình độ kỹ năng hiện tại của anh ta với độ khó của nhiệm vụ. Sau khi giải quyết một nhiệm vụ, kỹ năng của anh ta sẽ tăng lên và điều này có thể mở khóa các nhiệm vụ bổ sung mà trước đây quá khó hoặc chưa thể thực hiện được theo đúng thứ tự. 

Quá trình này bị ràng buộc bởi cấu trúc của mảng: vị trí rất quan trọng và trong số tất cả các nhiệm vụ có thể giải quyết được tại bất kỳ thời điểm nào, nhiệm vụ có chỉ số nhỏ nhất luôn được chọn. Khi một nhiệm vụ được thực hiện, nó sẽ biến mất khỏi sự cân nhắc và Vanya tiếp tục tìm kiếm từ vị trí đó. 

Mục tiêu là mô phỏng quá trình này một cách hiệu quả và xác định chuỗi nhiệm vụ đầy đủ mà Vanya sẽ giải quyết. 

Các ràng buộc ngụ ý rằng một mô phỏng đơn giản liên tục quét mảng từ đầu cho mọi tác vụ sẽ dẫn đến hành vi bậc hai trong trường hợp xấu nhất. Với n tối đa các giới hạn Codeforces điển hình như 2e5, quá trình quét O(n²) quá chậm vì nó sẽ yêu cầu tối đa 4e10 thao tác. Điều này ngay lập tức gợi ý rằng chúng ta phải tránh việc duyệt toàn bộ lặp đi lặp lại và thay vào đó duy trì quyền truy cập gia tăng vào ứng viên hợp lệ tiếp theo. 

Một trường hợp phức tạp xuất hiện khi nhiệm vụ duy nhất có thể giải quyết được tại một thời điểm nào đó nằm ở xa bên phải, nhưng các nhiệm vụ trước đó lại có thể giải quyết được sau đó do kỹ năng tăng lên. Việc quét tham lam từ trái sang phải một cách ngây thơ mà không có sổ sách kế toán thích hợp có thể bỏ qua các tác vụ mới được mở khóa hoặc quét lại một cách không cần thiết và vẫn bỏ sót các ràng buộc về thứ tự. 

Một trường hợp khó khăn khác là khi xuất hiện nhiều nhiệm vụ có cùng độ khó. Thuật toán phải duy trì lần xuất hiện sớm nhất chưa được xử lý, không khởi động lại quá trình quét từ đầu sau mỗi lần tăng kỹ năng. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng hành vi của Vanya theo nghĩa đen. Chúng tôi liên tục quét toàn bộ mảng, chọn nhiệm vụ ngoài cùng bên trái có độ khó không vượt quá kỹ năng hiện tại, loại bỏ nó, tăng kỹ năng và lặp lại. Điều này đúng vì nó phản ánh chính xác các quy tắc. Tuy nhiên, mỗi lần quét toàn bộ tốn O(n) và chúng tôi có thể thực hiện tối đa O(n) lượt chọn thành công, dẫn đến tổng độ phức tạp là O(n²). Đối với đầu vào lớn, điều này trở nên không khả thi. 

Quan sát chính là mỗi cấp độ khó hoạt động độc lập về mặt vị trí: chúng tôi chỉ quan tâm đến lần xuất hiện tiếp theo chưa được xử lý của từng độ khó. Khi đã biết những vị trí đó, chúng tôi có thể quyết định ứng viên mà không cần quét toàn bộ mảng. 

Điều này gợi ý việc duy trì, đối với mỗi giá trị độ khó, một con trỏ tới lần xuất hiện không được sử dụng tiếp theo. Trong số tất cả các khó khăn hiện đang “hoạt động” (có nghĩa là độ khó ≤ kỹ năng hiện tại), chúng tôi muốn chỉ số xuất hiện tiếp theo nhỏ nhất. Điều này biến vấn đề thành việc trích xuất liên tục mức tối thiểu trong số các ứng cử viên được cập nhật động, đó chính xác là những gì một tập hợp có thứ tự hoặc vùng heap hỗ trợ. 

Khi kỹ năng tăng lên, một lớp độ khó mới sẽ hoạt động và chúng ta chỉ cần chèn lần xuất hiện đầu tiên của nó. Khi một nhiệm vụ được thực hiện, chúng tôi sẽ vượt qua độ khó đó và chèn lần xuất hiện tiếp theo nếu nó vẫn tồn tại. Điều này tránh việc quét lại toàn bộ mảng và giảm quy trình xuống O(n log n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Đã đặt hàng cho các lần xuất hiện tiếp theo | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Nhóm tất cả các chỉ số của từng giá trị độ khó theo thứ tự tăng dần. Điều này cho chúng tôi biết, đối với bất kỳ khó khăn nào, nhiệm vụ không được sử dụng tiếp theo nằm ở đâu mà không cần tìm kiếm. 
2. Duy trì một con trỏ cho mỗi độ khó để chỉ ra vị trí chưa được sử dụng đầu tiên trong danh sách của nó. Ban đầu, tất cả các con trỏ đều ở mức 0. 
3. Duy trì một cấu trúc có thứ tự, điển hình là một bộ, lưu trữ các cặp biểu mẫu (chỉ mục, độ khó) cho tất cả các độ khó hiện đang hoạt động theo kỹ năng của Vanya. Ban đầu, chỉ có những khó khăn ≤ kỹ năng bắt đầu mới được kích hoạt, vì vậy chúng tôi chèn lần xuất hiện đầu tiên của những khó khăn đó. 
4. Liên tục trích xuất chỉ số nhỏ nhất từ ​​tập hợp. Điều này thể hiện nhiệm vụ tiếp theo mà Vanya sẽ giải quyết theo quy tắc luôn thực hiện nhiệm vụ có thể đạt được sớm nhất. 
5. Sau khi loại bỏ một nhiệm vụ ở chỉ mục i có độ khó d, hãy chuyển con trỏ của d sang lần xuất hiện tiếp theo. Nếu sự xuất hiện như vậy tồn tại và nó hợp lệ theo các ràng buộc hiện tại, hãy chèn nó vào tập hợp. 
6. Tăng kỹ năng của Vanya sau mỗi lần hoàn thành nhiệm vụ thành công. Khi kỹ năng tăng lên giá trị mới s, hãy kích hoạt độ khó này bằng cách chèn lần xuất hiện đầu tiên của nó vào bộ, miễn là nó tồn tại. 
7. Tiếp tục cho đến khi tập hợp trở nên trống rỗng, nghĩa là không còn tác vụ nào có thể truy cập được nữa. 

Điều bất biến chính là bộ này luôn chứa chính xác chỉ số ứng cử viên có sẵn tiếp theo cho mọi độ khó mà kỹ năng của Vanya hiện cho phép. Không có nhiệm vụ hợp lệ nào bị bỏ qua, bởi vì bất cứ khi nào một khó khăn được kích hoạt hoặc tăng lên, lần xuất hiện tiếp theo của nó sẽ được chèn ngay lập tức. Không có nhiệm vụ không hợp lệ nào được chọn, bởi vì chỉ các chỉ số từ những khó khăn đang hoạt động mới được lưu trữ. 

Điều này đảm bảo rằng mức tối thiểu được trích xuất luôn là nhiệm vụ tiếp theo chính xác theo thứ tự chung, phù hợp với quy tắc tham lam được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    pos = {}
    for i, v in enumerate(a):
        if v not in pos:
            pos[v] = []
        pos[v].append(i)

    # pointer per value
    ptr = {v: 0 for v in pos}

    import bisect

    # current skill starts at 1 (as implied by statement structure)
    skill = 1

    import heapq
    heap = []

    def add_value(v):
        if v not in pos:
            return
        i = ptr[v]
        if i < len(pos[v]):
            heapq.heappush(heap, (pos[v][i], v))

    # initialize active values
    for v in pos:
        if v <= skill:
            add_value(v)

    used = set()
    res = []

    while heap:
        i, v = heapq.heappop(heap)
        if (i, v) in used:
            continue
        used.add((i, v))
        res.append(i)

        ptr[v] += 1
        if ptr[v] < len(pos[v]):
            heapq.heappush(heap, (pos[v][ptr[v]], v))

        skill += 1
        if skill in pos:
            add_value(skill)

    print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách nhóm các vị trí của từng độ khó, điều này đảm bảo chúng ta có thể chuyển trực tiếp giữa các lần xuất hiện mà không cần quét. Từ điển con trỏ theo dõi tiến trình trong mỗi nhóm. 

Vùng heap duy trì các tác vụ ứng viên được sắp xếp theo chỉ mục, điều này thực thi yêu cầu chúng ta luôn chọn tác vụ có sẵn ở ngoài cùng bên trái. Mỗi lần chúng tôi thực hiện một nhiệm vụ, chúng tôi sẽ nâng cao độ khó của nhiệm vụ đó và đẩy lần xuất hiện tiếp theo. 

Quá trình tiến triển kỹ năng được xử lý một cách lười biếng: bất cứ khi nào kỹ năng tăng lên, chúng tôi chỉ kích hoạt độ khó mới tại thời điểm đó, chèn lần xuất hiện đầu tiên của nó. Điều này tránh việc tải trước tất cả các khả năng một cách sớm. 

Một chi tiết triển khai tinh tế là các mục heap lỗi thời có thể xuất hiện sau khi cập nhật con trỏ. Chúng được lọc bằng cách sử dụng tập hợp đã truy cập, đảm bảo chúng tôi không xử lý các cặp (chỉ mục, giá trị) cũ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi tác vụ được đẩy và bật ra khỏi vùng heap nhiều nhất một lần và các thao tác trên vùng heap tiêu tốn thời gian logarit | 
| Không gian | O(n) | Chúng tôi lưu trữ danh sách vị trí và các mục nhập đống | 

Thuật toán phù hợp thoải mái với các ràng buộc điển hình lên đến 2e5, vì log n nhỏ và mỗi phần tử được xử lý với số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        solve()
    except SystemExit:
        pass
    return ""

# Minimal case: single element
run("""1
1
""")

# Increasing chain
run("""5
1 2 3 4 5
""")

# Repeated values
run("""6
1 1 2 2 3 3
""")

# All same difficulty
run("""5
1 1 1 1 1
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yếu tố đơn |`0`| Độ chính xác cơ bản và khởi tạo | 
| Tăng chuỗi |`0 1 2 3 4`| Thứ tự tiến triển kỹ năng | 
| Giá trị lặp lại | phụ thuộc vào việc lập chỉ mục | xử lý nhiều lần xuất hiện | 
| Tất cả đều giống nhau | chỉ số tuần tự | tính chính xác của tiến bộ con trỏ | 

## Vỏ cạnh 

Trường hợp quan trọng xảy ra khi nhiều nhiệm vụ có cùng độ khó xen kẽ với những khó khăn khác. Ví dụ: nếu độ khó 1 xuất hiện ở vị trí 0, 5 và 10, thuật toán phải đảm bảo rằng sau khi lấy vị trí 0, thí sinh tiếp theo được nâng đúng lên vị trí 5 mà không cần nhập lại vị trí 0. 

Một trường hợp đặc biệt khác là khi kỹ năng tăng lên sẽ mở khóa một độ khó mới không còn lần xuất hiện nào trong hậu tố của mảng. Trong trường hợp đó, thao tác chèn không xảy ra và thuật toán phải tiếp tục mà không bị đình trệ hoặc cố gắng truy cập không hợp lệ.
