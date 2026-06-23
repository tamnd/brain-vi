---
title: "CF 105544M - Lập lịch tác vụ"
description: "Mỗi trường hợp thử nghiệm mô tả một hệ thống lập kế hoạch rất nhỏ nhận danh sách các nhiệm vụ. Mỗi tác vụ đều có một mã định danh và giá trị ưu tiên, đồng thời hệ thống phải quyết định thứ tự thực hiện các tác vụ."
date: "2026-06-22T23:39:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105544
codeforces_index: "M"
codeforces_contest_name: "The 2023 ICPC Asia Taoyuan Regional Programming Contest"
rating: 0
weight: 105544
solve_time_s: 56
verified: true
draft: false
---

[CF 105544M - Trình lập lịch tác vụ](https://codeforces.com/problemset/problem/105544/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả một hệ thống lập kế hoạch rất nhỏ nhận danh sách các nhiệm vụ. Mỗi tác vụ đều có một mã định danh và giá trị ưu tiên, đồng thời hệ thống phải quyết định thứ tự thực hiện các tác vụ. 

Chi tiết quan trọng là các nhiệm vụ đã được đưa ra theo thứ tự chúng được đưa vào hệ thống. Dòng thứ hai liệt kê các ID tác vụ theo thứ tự chèn đó và dòng thứ ba cung cấp mức độ ưu tiên tương ứng cho từng tác vụ. Lập kế hoạch luôn chọn nhiệm vụ có giá trị ưu tiên nhỏ nhất trước tiên. Nếu nhiều tác vụ có cùng mức độ ưu tiên thì hệ thống sẽ ngắt mối liên kết bằng cách chọn tác vụ đã được chèn trước đó. 

Vì vậy, đầu ra chỉ đơn giản là một hoán vị của các ID tác vụ đầu vào, được sắp xếp theo thứ tự chúng sẽ được thực thi theo quy tắc này. 

Những hạn chế là cực kỳ nhỏ. Với tối đa 100 tác vụ cho mỗi trường hợp kiểm thử và nhiều nhất là 10 trường hợp kiểm thử, ngay cả phương pháp sắp xếp O(n^2) cũng không đáng kể để chạy trong giới hạn. Điều này ngay lập tức cho chúng tôi biết rằng giải pháp không yêu cầu bất kỳ cấu trúc dữ liệu nâng cao hoặc tối ưu hóa nào ngoài việc sắp xếp tiêu chuẩn. 

Một trường hợp phức tạp xuất hiện khi nhiều nhiệm vụ có cùng mức độ ưu tiên. Trong trường hợp đó, tính chính xác phụ thuộc hoàn toàn vào việc giữ nguyên thứ tự chèn ban đầu. Ví dụ: nếu đầu vào là 

Mã số: 7 3 9 

Ưu tiên: 5 1 5 

thì nhiệm vụ 3 phải được thực hiện trước do mức độ ưu tiên 1, và từ 7 đến 9 (cả hai đều có mức ưu tiên 5), 7 phải đến trước 9 vì nó xuất hiện sớm hơn. Một cách tiếp cận ngây thơ chỉ sắp xếp theo mức độ ưu tiên sẽ hoán đổi không chính xác 7 và 9. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để mô phỏng bộ lập lịch là quét liên tục danh sách và chọn nhiệm vụ còn lại có mức độ ưu tiên nhỏ nhất. Mỗi lần chúng tôi chọn một nhiệm vụ, chúng tôi đánh dấu nó là đã xóa và tiếp tục. Điều này đúng vì nó tuân theo định nghĩa của bộ lập lịch một cách chính xác. 

Tuy nhiên, mỗi lựa chọn đều yêu cầu quét tất cả các tác vụ còn lại để tìm ra mức độ ưu tiên tối thiểu. Với n nhiệm vụ, điều này có nghĩa là có n lựa chọn, mỗi lựa chọn có giá O(n), dẫn đến O(n^2) cho mỗi trường hợp thử nghiệm. Mặc dù n ở đây chỉ là 100, cách tiếp cận này là công việc không cần thiết và trở nên vụng về về mặt khái niệm khi mở rộng đến các ràng buộc lớn hơn. 

Quan sát chính là bộ lập lịch tương đương với việc sắp xếp các tác vụ theo khóa tổng hợp. Khóa chính là mức độ ưu tiên và khóa phụ là thứ tự chèn. Khi chúng tôi đính kèm chỉ mục gốc cho từng tác vụ, chúng tôi có thể sắp xếp tất cả các tác vụ trong một lần sử dụng hai trường này. Hoạt động sắp xếp giải quyết tất cả các quyết định lập kế hoạch một cách tự nhiên vì mọi so sánh giữa hai nhiệm vụ đều được xác định đầy đủ bởi các quy tắc này. 

Vì vậy, vấn đề giảm xuống còn việc xây dựng danh sách bộ ba và thực hiện sắp xếp ổn định theo (mức độ ưu tiên, chỉ mục). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lựa chọn vũ lực | O(n^2) | O(n) | Đã chấp nhận | 
| Sắp xếp theo (ưu tiên, chỉ mục) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc danh sách ID nhiệm vụ và danh sách ưu tiên. Ghép nối từng nhiệm vụ với chỉ mục chèn của nó, vì chỉ mục đó thể hiện quy tắc ràng buộc. 
2. Xây dựng cấu trúc cho từng tác vụ lưu trữ mức độ ưu tiên, chỉ mục và ID của nó. Chỉ mục này rất quan trọng vì nó mã hóa thứ tự xuất hiện ban đầu. 
3. Sắp xếp tất cả các nhiệm vụ bằng cách sử dụng phép so sánh trước tiên xem xét mức độ ưu tiên và sau đó là chỉ mục. Chỉ số này đảm bảo rằng trong số những ưu tiên ngang nhau, những nhiệm vụ trước đó sẽ được ưu tiên sớm hơn. 
4. Sau khi sắp xếp, trích xuất ID nhiệm vụ theo thứ tự và xuất chúng. 

Lý do hoạt động sắp xếp ở đây là vì mọi quyết định lập kế hoạch đều độc lập khi các nhiệm vụ được xếp hạng toàn cầu theo hai tiêu chí này. Không có sự thay đổi linh hoạt nào về mức độ ưu tiên hoặc tập hợp nhiệm vụ, do đó thứ tự chung sẽ xác định đầy đủ thứ tự thực hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    out = []
    
    for _ in range(T):
        n = int(input())
        ids = list(map(int, input().split()))
        pr = list(map(int, input().split()))
        
        tasks = []
        for i in range(n):
            tasks.append((pr[i], i, ids[i]))
        
        tasks.sort()
        
        res = [str(task[2]) for task in tasks]
        out.append(" ".join(res))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên gắn mỗi tác vụ với chỉ mục của nó để thứ tự chèn được giữ nguyên dưới dạng khóa sắp xếp phụ. Sắp xếp bộ dữ liệu`(priority, index, id)`trực tiếp mã hóa cả hai quy tắc lập kế hoạch. Sau khi sắp xếp, chỉ có ID được trích xuất cho đầu ra. 

Một lỗi phổ biến ở đây là quên chỉ mục trong khóa sắp xếp. Khả năng sắp xếp của Python ổn định, nhưng việc dựa vào tính ổn định mà không bao gồm chỉ mục một cách rõ ràng sẽ rất dễ hỏng nếu cấu trúc bị sửa đổi hoặc nếu sử dụng một ngôn ngữ khác. Sắp xếp rõ ràng theo`(priority, index)`đảm bảo tính đúng đắn. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp thử nghiệm với các nhiệm vụ: 

Mã số: 4 1 3 2 

Ưu tiên: 2 1 2 1 

Chúng tôi xây dựng các bộ dữ liệu như`(priority, index, id)`: 

| chỉ mục | id | ưu tiên | bộ dữ liệu | 
| --- | --- | --- | --- | 
| 0 | 4 | 2 | (2, 0, 4) | 
| 1 | 1 | 1 | (1, 1, 1) | 
| 2 | 3 | 2 | (2, 2, 3) | 
| 3 | 2 | 1 | (1, 3, 2) | 

Sau khi sắp xếp: 

| vị trí | bộ dữ liệu | 
| --- | --- | 
| 0 | (1, 1, 1) | 
| 1 | (1, 3, 2) | 
| 2 | (2, 0, 4) | 
| 3 | (2, 2, 3) | 

Đầu ra trở thành:`1 2 4 3`. 

Dấu vết này cho thấy mức độ ưu tiên bằng nhau được giải quyết nghiêm ngặt theo chỉ mục như thế nào, đảm bảo thứ tự chèn ổn định trong mỗi nhóm ưu tiên. 

Bây giờ hãy xem xét một trường hợp đơn giản hơn: 

Mã số: 10 20 30 

Ưu tiên: 5 5 5 

Tất cả các tác vụ đều có cùng mức độ ưu tiên nên thứ tự phụ thuộc hoàn toàn vào chỉ mục chèn. Thứ tự sắp xếp vẫn giữ nguyên`10 20 30`, xác nhận rằng thuật toán suy biến thành thứ tự nhận dạng khi mức độ ưu tiên bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | Sắp xếp n nhiệm vụ theo (mức độ ưu tiên, chỉ mục) chiếm ưu thế | 
| Không gian | O(n) | Lưu trữ các bộ dữ liệu nhiệm vụ | 

Với n nhiều nhất là 100 và T nhiều nhất là 10 thì số phép toán tối đa là không đáng kể. Ngay cả cách tiếp cận bạo lực O(n^2) cũng sẽ nhanh, nhưng việc sắp xếp đơn giản và mạnh mẽ hơn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    from sys import stdout
    import sys
    
    input = sys.stdin.readline
    T = int(input())
    out = []
    
    for _ in range(T):
        n = int(input())
        ids = list(map(int, input().split()))
        pr = list(map(int, input().split()))
        
        tasks = [(pr[i], i, ids[i]) for i in range(n)]
        tasks.sort()
        out.append(" ".join(str(t[2]) for t in tasks))
    
    return "\n".join(out)

# provided sample style case
assert run("""1
3
0 1 2
2 0 0
""") == "1 2 0"

# all equal priorities
assert run("""1
5
5 4 3 2 1
10 10 10 10 10
""") == "5 4 3 2 1"

# already sorted
assert run("""1
4
1 2 3 4
1 2 3 4
""") == "1 2 3 4"

# reverse priorities
assert run("""1
4
1 2 3 4
4 3 2 1
""") == "4 3 2 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mọi ưu tiên như nhau | ổn định theo chỉ số | sự đúng đắn của sự ràng buộc | 
| đã được sắp xếp | bảo tồn danh tính | không sắp xếp lại không cần thiết | 
| ưu tiên đảo ngược | thứ tự ưu tiên nghiêm ngặt | tính chính xác của khóa chính | 

## Vỏ cạnh 

Khi tất cả các mức độ ưu tiên giống hệt nhau, thuật toán sẽ hoàn toàn dựa vào chỉ mục được lưu trữ. Mỗi bộ dữ liệu trở thành`(p, i, id)`bằng nhau`p`, do đó việc sắp xếp giảm xuống việc sắp xếp theo thứ tự`i`. Đầu ra khớp chính xác với trình tự chèn ban đầu, đây là hành vi bắt buộc. 

Khi một tác vụ có mức độ ưu tiên thấp nhất xuất hiện ở cuối đầu vào, việc sắp xếp vẫn đặt nó lên đầu tiên vì trường ưu tiên chiếm ưu thế so sánh. Chỉ mục chỉ quan trọng khi mức độ ưu tiên khớp nhau, do đó, các nhiệm vụ có mức độ ưu tiên thấp đến muộn sẽ nhảy lên trước các nhiệm vụ có mức độ ưu tiên cao trước đó một cách chính xác.
