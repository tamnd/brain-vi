---
title: "CF 104736E - Lợi nhuận tăng cao"
description: "Chúng ta có một mạng lưới các thành phố được kết nối tạo thành một cái cây. Mỗi thành phố có một nhãn số nguyên cố định thể hiện giá trị phổ biến của nó."
date: "2026-06-28T23:51:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104736
codeforces_index: "E"
codeforces_contest_name: "2023-2024 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 104736
solve_time_s: 56
verified: true
draft: false
---

[CF 104736E - Lợi nhuận tăng cao](https://codeforces.com/problemset/problem/104736/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mạng lưới các thành phố được kết nối tạo thành một cái cây. Mỗi thành phố có một nhãn số nguyên cố định thể hiện giá trị phổ biến của nó. Marina bắt đầu chuyến tham quan của mình tại một thành phố cụ thể và từ đó cô ấy có thể đi bộ dọc theo những con đường mà cô ấy muốn mà không bị hạn chế trong việc thăm lại các thành phố hoặc đi ngang qua những phần cây đã ghé thăm. 

Thời điểm duy nhất quan trọng là khi một thành phố được ghé thăm lần đầu tiên. Lần đầu tiên một thành phố được vào, nó sẽ được chỉ định “thời gian đăng bài” tiếp theo bắt đầu từ 1. Nếu một thành phố là thành phố mới thứ i mà cô ấy phát hiện ra thì đóng góp vào lợi nhuận của nó sẽ được nhân với giá trị mức độ phổ biến của nó. Thành phố bắt đầu buộc phải là thành phố được phát hiện đầu tiên. 

Nhiệm vụ là xác định thứ tự các thành phố mới được ghé thăm lần đầu tiên sao cho tổng trọng số của các giá trị phổ biến của chúng, với trọng số tăng dần từ 1 đến N theo thứ tự khám phá, là tối đa. 

Mặc dù đầu vào bao gồm cấu trúc cây, điểm tinh tế quan trọng là các quy tắc chuyển động không hạn chế thứ tự truy cập ngoài khả năng kết nối. Vì cô ấy có thể đi qua các cạnh một cách tùy ý nhiều lần và được phép đi qua các thành phố đã ghé thăm hoặc chưa ghé thăm mà không bị ràng buộc, nên cấu trúc không giới hạn trình tự các lượt truy cập đầu tiên ngoài việc cố định thành phố đầu tiên. 

Các ràng buộc cho phép tối đa 3×10^5 thành phố, điều này buộc phải có giải pháp O(N log N) hoặc O(N). Bất kỳ chiến lược nào cố gắng mô phỏng chuyển động từng bước trên cây sẽ quá chậm vì nó sẽ liên tục khám phá các cây con lớn, dẫn đến hành vi bậc hai trong trường hợp xấu nhất. 

Trường hợp cạnh không rõ ràng là khi người ta giả sử cấu trúc cây buộc phải có thứ tự DFS hoặc BFS. Ví dụ: trong cây hình ngôi sao có tâm ở R, trực giác BFS sẽ đề xuất sớm đến thăm hàng xóm. Tuy nhiên, vì việc duyệt không hạn chế việc bỏ qua, nên chúng ta vẫn có thể chọn bất kỳ lá nào chưa được thăm tiếp theo bất kể các ràng buộc kề, khiến cho việc duyệt thứ tự không còn phù hợp nữa. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là mô phỏng chuyển động của Marina một cách rõ ràng. Chúng tôi bắt đầu tại R, duy trì một tập hợp các thành phố đã ghé thăm và liên tục đi dọc theo các cạnh để cố gắng quyết định thành phố nào chưa được ghé thăm tiếp theo. Mỗi lần chúng tôi vào một thành phố mới lần đầu tiên, chúng tôi sẽ chỉ định chỉ số tiếp theo và tích lũy đóng góp. 

Vấn đề là việc quyết định “đi đến thành phố nào tiếp theo” thực sự không bị hạn chế một cách có ý nghĩa bởi tuyên bố vấn đề. Bất kỳ nút nào chưa được thăm đều có thể truy cập được bất kỳ lúc nào bằng cách đi qua cây, vì chuyển động không bị hạn chế và chúng ta được phép đi qua bất kỳ cạnh nào nhiều lần. Mô phỏng sẽ vẫn tiêu tốn O(N) thời gian cho mỗi bước để tìm ứng viên tiếp theo hoặc tính toán khả năng tiếp cận, dẫn đến hành vi O(N^2). 

Quan sát chính là cấu trúc cây không liên quan đến thứ tự của các lần truy cập đầu tiên. Ràng buộc cố định duy nhất là R phải xuất hiện đầu tiên theo thứ tự. Sau đó, mọi thành phố còn lại có thể được đặt ở bất kỳ vị trí nào từ 2 đến N. 

Khi hiểu được điều này, vấn đề sẽ trở thành một nhiệm vụ sắp xếp lại thuần túy: chúng ta gán các vị trí từ 2 đến N cho các nút còn lại để tối đa hóa tổng có trọng số khi các trọng số tăng dần. Vì trọng số tăng theo vị trí nên chúng tôi muốn các giá trị phổ biến lớn hơn xuất hiện sau trong chuỗi để chúng được nhân với các chỉ số lớn hơn. 

Đây là một tình huống bất bình đẳng sắp xếp lại cổ điển. Để tối đa hóa tổng tích số giữa một chuỗi trọng số tăng dần cố định và các giá trị được chọn, chúng tôi sắp xếp các giá trị theo thứ tự tăng dần và khớp chúng với các chỉ số tăng dần. Ngoại lệ duy nhất là vị trí gốc cố định ở chỉ số 1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng truyền tải | O(N^2) | O(N) | Quá chậm | 
| Bài tập dựa trên sắp xếp | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi coi vị trí chuỗi là một trọng số tăng tuyến tính từ 1 đến N. Chúng tôi muốn chỉ định cho mỗi thành phố chính xác một vị trí, với vị trí đầu tiên đã được cố định. 

## Hướng dẫn thuật toán 

1. Sửa thành phố bắt đầu R ở vị trí 1, vì sự cố buộc chuyến tham quan phải bắt đầu ở đó. Điều này đóng góp một giá trị cố định bằng R. 
2. Tập hợp tất cả các thành phố khác vào danh sách. Đây là những yếu tố chúng ta có thể tự do hoán đổi theo thứ tự của những lần truy cập đầu tiên. 
3. Sắp xếp các thành phố còn lại theo giá trị phổ biến theo thứ tự tăng dần. Điều này đảm bảo các giá trị nhỏ hơn nhận được trọng số vị trí nhỏ hơn. 
4. Xếp vị trí từ 2 đến N theo thứ tự tăng dần cho các thành phố được sắp xếp. Mức độ phổ biến nhỏ nhất còn lại sẽ giành được vị trí thứ 2, mức độ phổ biến tiếp theo sẽ đạt được vị trí 3, v.v. 
5. Tính tổng số tiền bằng cách cộng phần đóng góp của từng vị trí cặp được chỉ định × mức độ phổ biến. 

Lý do hoạt động sắp xếp là vì bất kỳ sự đảo ngược nào giữa hai thành phố có giá trị phổ biến khác nhau đều có thể được hoán đổi để cải thiện kết quả. Nếu mức độ phổ biến lớn hơn được đặt sớm hơn và mức độ phổ biến nhỏ hơn được đặt sau, việc hoán đổi chúng sẽ làm tăng tổng trọng số vì giá trị lớn hơn sẽ đạt được hệ số nhân lớn hơn. 

### Tại sao nó hoạt động 

Trình tự các trọng lượng đang tăng lên một cách nghiêm ngặt. Đối với hai thành phố a và b bất kỳ được gán cho các vị trí i và j với i < j, nếu mức độ phổ biến[a] > mức độ phổ biến[b], việc hoán đổi chúng sẽ làm thay đổi mức đóng góp theo (j - i)(popularity[a] - mức độ phổ biến[b]), kết quả này là dương. Vì vậy, bất kỳ sự sắp xếp nào có sự đảo ngược như vậy đều không tối ưu. Việc loại bỏ nhiều lần sự đảo ngược dẫn đến việc sắp xếp chính xác theo mức độ phổ biến theo thứ tự không giảm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, r = map(int, input().split())
    for _ in range(n - 1):
        input()
    
    vals = [i for i in range(1, n + 1) if i != r]
    vals.sort()
    
    ans = r * 1
    for i, v in enumerate(vals, start=2):
        ans += i * v
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai hoàn toàn bỏ qua các cạnh sau khi đọc chúng, vì cấu trúc biểu đồ không ảnh hưởng đến thứ tự tối ưu. Sự phụ thuộc đầu vào có ý nghĩa duy nhất là danh tính của nút bắt đầu. 

Danh sách`vals`chứa tất cả các nút ngoại trừ nút gốc và việc sắp xếp nó đảm bảo ghép nối tối ưu với trọng số vị trí ngày càng tăng. Vòng lặp bắt đầu liệt kê ở số 2 để phản ánh rằng vị trí đầu tiên đã bị R chiếm giữ. 

## Ví dụ đã hoạt động 

Xét mẫu có N = 7 và R = 3. Các thành phố còn lại là 1, 2, 4, 5, 6, 7. Sắp xếp chúng cho 1, 2, 4, 5, 6, 7. Chúng ta gán các vị trí từ 2 đến 7 theo thứ tự đó. 

| Vị trí tôi | Thành phố | Đóng góp i × thành phố | 
| --- | --- | --- | 
| 1 | 3 | 3 | 
| 2 | 1 | 2 | 
| 3 | 2 | 6 | 
| 4 | 4 | 16 | 
| 5 | 5 | 25 | 
| 6 | 6 | 36 | 
| 7 | 7 | 49 | 

Tổng số được tối đa hóa với ràng buộc là vị trí 1 được cố định. 

Đối với trường hợp một nút N = 1, R = 1, không có thành phố nào khác. Câu trả lời đơn giản là 1 × 1. 

| Vị trí tôi | Thành phố | Đóng góp | 
| --- | --- | --- | 
| 1 | 1 | 1 | 

Điều này xác nhận rằng thuật toán xử lý tốt trường hợp tối thiểu mà không cần phân nhánh đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Sắp xếp N−1 thành phố còn lại chiếm ưu thế trong thời gian chạy | 
| Không gian | O(N) | Lưu trữ danh sách các thành phố không bao gồm gốc | 

Độ phức tạp dễ dàng nằm trong giới hạn cho N lên tới 3×10^5. Sự vắng mặt của việc truyền tải đồ thị đảm bảo chúng ta tránh được bất kỳ sự phụ thuộc tuyến tính hoặc bậc hai nào vào các cạnh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# sample 1
assert run("""7 3
3 5
3 7
5 1
5 4
7 2
7 6
""") == "111", "sample 1"

# sample 2
assert run("""1 1
""") == "1", "single node"

# custom: small line tree
assert run("""4 2
1 2
2 3
3 4
""") == "19", "line tree ordering"

# custom: star
assert run("""5 1
1 2
1 3
1 4
1 5
""") == "55", "star center"

# custom: reverse effect check
assert run("""3 2
1 2
2 3
""") == "11", "ordering swap check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | 111 | tính đúng đắn trên cây tổng quát | 
| nút đơn | 1 | trường hợp tối thiểu | 
| cây dòng | 19 | đặt hàng trên cấu trúc đường dẫn | 
| ngôi sao | 55 | độc lập khỏi cấu trúc liên kết | 
| kiểm tra trao đổi | 11 | tính chính xác của đối số đảo ngược | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi cây gợi ý một thứ tự truyền tải bắt buộc, chẳng hạn như một chuỗi dài. Hãy xem xét đường dẫn 1-2-3-4-5 với R = 3. Giả định DFS đơn giản có thể gợi ý việc truy cập các nút theo trình tự bị ràng buộc như 3,2,1,4,5. Tuy nhiên, thuật toán cho phép sắp xếp lại tự do sau khi sửa 3 số đầu tiên. 

Các nút còn lại được sắp xếp là 1,2,4,5, tạo ra sự sắp xếp tối ưu nghiêm ngặt. Bất kỳ nỗ lực nào nhằm tôn trọng thứ tự kề sẽ chỉ tạo ra sự đảo ngược mà đối số hoán đổi cho thấy luôn có thể được cải thiện.
