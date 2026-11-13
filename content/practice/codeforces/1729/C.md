---
title: "CF 1729C - Nhảy Gạch"
description: "Chúng ta được cấp một chuỗi các chữ cái viết thường đại diện cho một dòng gạch. Mỗi vị trí là một nút trong đường dẫn từ ký tự đầu tiên đến ký tự cuối cùng nhưng chúng ta không bắt buộc phải di chuyển chỉ đến các vị trí liền kề."
date: "2026-06-15T02:30:33+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "strings"]
categories: ["algorithms"]
codeforces_contest: 1729
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 820 (Div. 3)"
rating: 1100
weight: 1729
solve_time_s: 398
verified: false
draft: false
---

[CF 1729C - Nhảy trên gạch](https://codeforces.com/problemset/problem/1729/C) 

**Đánh giá:** 1100 
**Tas:** thuật toán xây dựng, chuỗi 
**Thời gian giải:** 6 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các chữ cái viết thường đại diện cho một dòng gạch. Mỗi vị trí là một nút trong đường dẫn từ ký tự đầu tiên đến ký tự cuối cùng nhưng chúng ta không bắt buộc phải di chuyển chỉ đến các vị trí liền kề. Thay vào đó, chúng ta có thể nhảy từ bất kỳ chỉ mục nào sang bất kỳ chỉ mục nào khác và chi phí của bước nhảy chỉ phụ thuộc vào các chữ cái: đó là sự khác biệt tuyệt đối giữa vị trí bảng chữ cái của chúng. 

Nhiệm vụ là xây dựng một đường dẫn hợp lệ bắt đầu từ chỉ mục 1 và kết thúc ở chỉ mục n, truy cập mỗi chỉ mục nhiều nhất một lần. Trong số tất cả các đường dẫn đạt được tổng chi phí tối thiểu có thể, chúng ta phải chọn một đường sử dụng số lượng chỉ mục tối đa, nghĩa là chúng ta muốn chèn càng nhiều điểm dừng trung gian càng tốt mà không làm tăng chi phí tối ưu. 

Vì vậy, bài toán có hai lớp: đầu tiên là giảm thiểu tổng chi phí, sau đó trong số tất cả các giải pháp chi phí tối thiểu sẽ tối đa hóa số đỉnh trên đường đi. 

Ràng buộc về tổng độ dài chuỗi trong các trường hợp thử nghiệm tối đa là 2e5 ngay lập tức gợi ý giải pháp O(n) hoặc O(n log n) cho mỗi trường hợp thử nghiệm. Bất kỳ phương trình bậc hai nào trong một chuỗi sẽ thất bại nếu một ca kiểm thử lớn. 

Trường hợp cạnh tinh tế xuất hiện khi có nhiều chữ cái nằm giữa các điểm cuối theo thứ tự bảng chữ cái. Ví dụ: trong một chuỗi như "az", bất kỳ chữ cái trung gian nào như "m" đều có chi phí phụ thuộc vào khoảng cách của nó với cả hai điểm cuối và việc chèn các nút bổ sung có thể duy trì hoặc tăng chi phí tùy thuộc vào cấu trúc. Một đường dẫn ngắn nhất ngây thơ trên tất cả các chỉ mục sẽ cho phép xem lại cấu trúc trung gian một cách không cần thiết, nhưng chúng tôi bị cấm xem lại các chỉ mục, vì vậy các chu kỳ không được phép. 

Khó khăn chính là chúng ta không chỉ tìm đường đi ngắn nhất trong biểu đồ có các cạnh tùy ý. Chúng ta phải khai thác cấu trúc theo trọng số cạnh để tránh xem xét chuyển đổi O(n^2). 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô hình hóa bài toán dưới dạng đường đi ngắn nhất trên đồ thị có hướng hoàn chỉnh với n nút, trong đó mỗi cặp chỉ số đều có một cạnh có trọng số. Chạy Dijkstra sẽ mang lại chi phí tối thiểu chính xác, nhưng với n lên tới 2e5 thì hành vi O(n^2 log n) là không thể. 

Ngay cả khi chúng tôi hạn chế chuyển đổi một cách khéo léo, chế độ xem đơn giản vẫn xem xét các cạnh O(n^2) có khả năng. Điểm nghẽn là mọi nút đều có thể kết nối với mọi nút khác với chi phí chỉ phụ thuộc vào chữ cái chứ không phải vị trí. 

Điều quan trọng cần lưu ý là chỉ có thứ tự tương đối của các ký tự trong bảng chữ cái mới quan trọng chứ không phải vị trí của chúng. Nếu chúng ta nghĩ về mặt chữ cái, bất kỳ chuyển động tối ưu nào cũng sẽ ưu tiên di chuyển giữa các giá trị chữ cái gần đó, bởi vì việc nhảy qua các khoảng trống lớn trong bảng chữ cái sẽ tốn kém và không thể phân tách thành các bước trung gian rẻ hơn trừ khi những bước trung gian đó tồn tại. 

Điều này gợi ý việc nén vấn đề bằng cách sắp xếp các chỉ mục dựa trên ký tự của chúng. Khi các chỉ số được nhóm theo chữ cái, chúng ta chỉ cần xem xét sự chuyển tiếp giữa các chữ cái liền kề theo thứ tự được sắp xếp. Trong cùng một chữ cái, tất cả các vị trí đều có chi phí tương đương nhau, vì vậy chúng ta có thể xâu chuỗi chúng một cách tự do để tăng số lượng nút truy cập mà không ảnh hưởng đến chi phí. 

Vì vậy, cấu trúc tối ưu trở thành một bước đi đơn điệu trên các giá trị ký tự, truy cập tất cả các chỉ số được nhóm theo các chữ cái, được sắp xếp sao cho chúng ta duyệt qua các chữ cái theo thứ tự tăng hoặc giảm của ranh giới tần số tùy thuộc vào điểm cuối. 

Cấu trúc đúng sẽ giảm xuống việc chọn hướng trong bảng chữ cái giữa s[1] và s[n], sau đó truy cập tất cả các chỉ mục có các ký tự nằm giữa chúng theo thứ tự ký tự được sắp xếp, nhóm các lần xuất hiện trên mỗi ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoàn thành đường đi ngắn nhất của đồ thị | O(n^2 log n) | O(n^2) | Quá chậm | 
| Sắp xếp/nhóm theo ký tự | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chuyển đổi từng ký tự sang chỉ mục bảng chữ cái của nó. Giả sử bắt đầu là s[1] và kết thúc là s[n]. 
2. Nếu phần đầu và phần cuối có cùng giá trị ký tự thì bất kỳ đường dẫn nào truy cập vào tất cả các lần xuất hiện của ký tự đó ở giữa đều có thể được sắp xếp lại mà không tăng chi phí. Đường dẫn tối ưu là truy cập tất cả các chỉ mục có ký tự đó, vì việc thêm các bước di chuyển có cùng chữ cái không làm tăng chi phí. 
3. Ngược lại, hãy xác định hướng di chuyển trong không gian bảng chữ cái. Nếu giá trị đầu nhỏ hơn giá trị cuối, chúng ta sẽ di chuyển lên trên; nếu không chúng ta sẽ di chuyển xuống dưới. Hướng này xác định những nhóm ký tự trung gian mà chúng tôi đưa vào. 
4. Thu thập tất cả các chỉ số được nhóm theo giá trị ký tự. Đối với mỗi ký tự, lưu trữ tất cả các vị trí nơi nó xuất hiện. 
5. Di chuyển các ký tự từ giá trị đầu đến giá trị cuối theo hướng đã chọn. Đối với mỗi ký tự nằm giữa chúng, hãy nối tất cả các chỉ mục của nó theo bất kỳ thứ tự nào. Thứ tự bên trong một nhóm ký tự không ảnh hưởng đến giá cả vì tất cả đều có giá trị chữ cái giống hệt nhau. 
6. Xây dựng đường dẫn cuối cùng như sau: bắt đầu chỉ số 1, sau đó thu thập tất cả các chỉ số trung gian theo thứ tự duyệt, rồi đến chỉ số cuối cùng n. 
7. Tính toán chi phí bằng cách tính tổng chênh lệch tuyệt đối giữa các giá trị ký tự liên tiếp dọc theo đường dẫn được xây dựng. 

Lý do thứ tự này hợp lệ là vì việc di chuyển theo thứ tự đơn điệu trong bảng chữ cái đảm bảo chúng ta không bao giờ "quay ngược" những khoảng trống chữ cái lớn hơn, điều này sẽ gây ra chi phí không cần thiết. 

### Tại sao nó hoạt động 

Hàm chi phí chỉ phụ thuộc vào các giá trị chữ cái và giống số liệu trên các số nguyên từ 1 đến 26. Bất kỳ đường đi tối ưu nào giúp giảm thiểu tổng chi phí đều không thể hưởng lợi từ việc dao động giữa các chữ cái ở xa vì mỗi đường vòng đều làm tăng chênh lệch tuyệt đối. Vì vậy, giải pháp tối ưu phải đơn điệu trong không gian bảng chữ cái giữa các điểm cuối. Khi tính đơn điệu được thực thi, việc chèn tất cả các lần xuất hiện trung gian của các ký tự sẽ không làm tăng chi phí vì các lượt truy cập lặp lại trong cùng một giá trị ký tự sẽ không góp phần thay đổi bảng chữ cái gia tăng khi được nhóm liên tiếp. Điều này đảm bảo cả chi phí tối thiểu và số lượng chỉ mục được truy cập tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        n = len(s)

        pos = [[] for _ in range(26)]
        for i, ch in enumerate(s):
            pos[ord(ch) - 97].append(i + 1)

        start = ord(s[0]) - 97
        end = ord(s[-1]) - 97

        path = [1]

        if start <= end:
            for c in range(start, end + 1):
                for idx in pos[c]:
                    if idx != 1 and idx != n:
                        path.append(idx)
        else:
            for c in range(start, end - 1, -1):
                for idx in pos[c]:
                    if idx != 1 and idx != n:
                        path.append(idx)

        path.append(n)

        cost = 0
        for i in range(len(path) - 1):
            cost += abs((ord(s[path[i] - 1]) - 97) - (ord(s[path[i + 1] - 1]) - 97))

        print(cost, len(path))
        print(*path)

if __name__ == "__main__":
    solve()
```Việc triển khai nhóm các chỉ mục theo ký tự trước tiên, điều này tránh mọi so sánh theo cặp giữa các vị trí. Hướng truyền tải được chọn dựa trên các ký tự điểm cuối, đảm bảo quét đơn điệu trong không gian bảng chữ cái. 

Chúng tôi loại trừ rõ ràng các chỉ số 1 và n khỏi phần chèn trung gian để tránh trùng lặp vì điểm cuối luôn cố định. 

Việc tính toán chi phí được thực hiện sau khi xây dựng để tránh làm phức tạp logic lựa chọn. Vì đường dẫn đã có cấu trúc tối ưu nên chúng tôi chỉ xác minh trọng số của nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`logic`Bắt đầu là`l`, kết thúc là`c`, vì vậy chúng ta duyệt xuống dưới theo thứ tự bảng chữ cái. 

| Bước | Phạm vi char hiện tại | Đã thêm chỉ số | Đường dẫn | 
| --- | --- | --- | --- | 
| ban đầu | - | 1 | 1 | 
| tôi nhóm | tôi | 1 (chỉ bắt đầu) | 1 | 
| k-j-i-h-g-f-e-d-c | giảm dần | 4, 3, 5 | 1 4 3 5 | 

Chi phí được tính bằng sự khác biệt giữa các giá trị chữ cái dọc theo gốc đơn điệu này, phù hợp với các chuyển đổi tối thiểu có thể có. 

Điều này chứng tỏ rằng các ký tự trung gian giữa các điểm cuối được sử dụng để tăng độ dài đường dẫn mà không vi phạm chi phí tối ưu. 

### Ví dụ 2:`codeforces`Bắt đầu là`c`, kết thúc là`s`, vì vậy chúng tôi di chuyển lên trên. 

| Bước | Phạm vi char hiện tại | Đã thêm chỉ số | Đường dẫn | 
| --- | --- | --- | --- | 
| ban đầu | - | 1 | 1 | 
| c đến s | tăng dần | tất cả các lần xuất hiện được nhóm | 1 8 3 4 9 5 2 6 7 10 | 

Điều này cho thấy rằng khi tồn tại nhiều chữ cái trung gian, chúng tôi khai thác triệt để tất cả các lần xuất hiện để tối đa hóa số lần nhảy trong khi vẫn duy trì tiến trình bảng chữ cái đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi chỉ mục được truy cập một lần trong quá trình nhóm và một lần trong quá trình xây dựng đường dẫn | 
| Không gian | O(n) | Lưu trữ danh sách vị trí và đường dẫn đầu ra | 

Tổng số n trên các trường hợp thử nghiệm tối đa là 2e5, do đó việc xử lý tuyến tính là đủ và an toàn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    import sys as _sys
    input = _sys.stdin.readline

    t = int(input())
    for _ in range(t):
        s = input().strip()
        n = len(s)

        pos = [[] for _ in range(26)]
        for i, ch in enumerate(s):
            pos[ord(ch) - 97].append(i + 1)

        start = ord(s[0]) - 97
        end = ord(s[-1]) - 97

        path = [1]

        if start <= end:
            for c in range(start, end + 1):
                for idx in pos[c]:
                    if idx != 1 and idx != n:
                        path.append(idx)
        else:
            for c in range(start, end - 1, -1):
                for idx in pos[c]:
                    if idx != 1 and idx != n:
                        path.append(idx)

        path.append(n)

        cost = 0
        for i in range(len(path) - 1):
            cost += abs((ord(s[path[i] - 1]) - 97) - (ord(s[path[i + 1] - 1]) - 97))

        print(cost, len(path))
        print(*path)

    return out.getvalue().strip()

# provided samples (format adapted as exact strings omitted for brevity)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`aaaa`|`0 4 ...`| tất cả các chữ cái giống nhau, mở rộng tối đa | 
|`ab`|`1 2 1 2`| trường hợp tăng đơn giản nhất | 
|`ba`|`1 2 1 2`| xử lý hướng ngược lại | 
|`abcba`| con đường đơn điệu có cấu trúc | chữ cái đối xứng và công tắc hướng | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi tất cả các ký tự giống hệt nhau. Thuật toán nhóm tất cả các chỉ số dưới một ký tự và xuất ra mọi vị trí trong khoảng từ 1 đến n. Vì tất cả các khác biệt về chữ cái đều bằng 0 nên chi phí vẫn bằng 0 trong khi tối đa hóa số lượng nút được truy cập. Việc xây dựng đương nhiên bao gồm tất cả các chỉ số theo thứ tự, đáp ứng cả hai mục tiêu. 

Một trường hợp cạnh khác là chuỗi giảm nghiêm ngặt như "cba". Thuật toán chọn di chuyển xuống dưới và bao gồm tất cả các chỉ mục theo đúng thứ tự bảng chữ cái đảo ngược. Mặc dù các vị trí không được sắp xếp trong chuỗi, việc nhóm đảm bảo mỗi lần xuất hiện được bao gồm chính xác một lần và chi phí vẫn ở mức tối thiểu vì mỗi bước chỉ di chuyển một đơn vị trong không gian bảng chữ cái. 

Trường hợp thứ ba liên quan đến các ký tự hỗn hợp như "abac". Việc truyền tải từ a đến c bao gồm cả nhóm 'a' và 'b', đảm bảo không bỏ qua chữ cái trung gian có lợi nào. Đường dẫn bao gồm tất cả các lần xuất hiện của 'a' và 'b', tối đa hóa các bước nhảy mà không đưa ra các chuyển đổi không đơn điệu sẽ làm tăng chi phí.
