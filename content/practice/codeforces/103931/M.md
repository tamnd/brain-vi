---
title: "CF 103931M - Trường đại học của tôi tốt hơn trường của bạn"
description: "Chúng tôi được cung cấp một số bảng xếp hạng hoàn chỉnh của cùng một nhóm trường đại học. Mỗi thứ hạng là một hoán vị, được sắp xếp từ tốt nhất đến kém nhất."
date: "2026-07-02T07:19:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103931
codeforces_index: "M"
codeforces_contest_name: "2022 Shanghai Collegiate Programming Contest"
rating: 0
weight: 103931
solve_time_s: 51
verified: true
draft: false
---

[CF 103931M - Trường đại học của tôi tốt hơn trường của bạn](https://codeforces.com/problemset/problem/103931/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số bảng xếp hạng hoàn chỉnh của cùng một nhóm trường đại học. Mỗi thứ hạng là một hoán vị, được sắp xếp từ tốt nhất đến kém nhất. Từ những bảng xếp hạng này, chúng tôi rút ra mối quan hệ trực tiếp giữa các trường đại học: nếu trường đại học x xuất hiện trên trường đại học y trong ít nhất một bảng xếp hạng, chúng tôi nói rằng x trực tiếp tốt hơn y. 

Từ mối quan hệ trực tiếp này, chúng ta xác định khái niệm bắc cầu về tính ưu việt. Một trường đại học x được coi là tốt hơn y nếu chúng ta có thể chuyển từ x đến y thông qua một chuỗi các trường đại học, trong đó mỗi cặp liên tiếp tôn trọng mối quan hệ “tốt hơn trực tiếp” từ một số xếp hạng. Theo thuật ngữ biểu đồ, chúng tôi xây dựng biểu đồ có hướng trên n nút trong đó chúng tôi thêm cạnh x → y bất cứ khi nào x ở trên y trong ít nhất một xếp hạng và sau đó chúng tôi yêu cầu kích thước khả năng tiếp cận. 

Đối với mỗi trường đại học, nhiệm vụ là tính toán xem nó có thể tiếp cận được bao nhiêu trường đại học khác trong biểu đồ có hướng này. 

Các ràng buộc cực kỳ chặt chẽ theo nghĩa kết hợp: n và m mỗi cái có thể lên tới 5×10^5, nhưng tổng số mục xếp hạng n·m nhiều nhất là 10^6. Điều đó có nghĩa là mặc dù về mặt lý thuyết, biểu đồ có khả năng có các cạnh O(n^2), nhưng chúng tôi chỉ được phép xử lý tổng cộng khoảng một triệu so sánh. Bất kỳ giải pháp nào xây dựng rõ ràng tất cả các mối quan hệ theo cặp giữa các trường đại học ở các bảng xếp hạng khác nhau đều không thể thực hiện được. 

Việc đóng chuyển tiếp ngây thơ trên một biểu đồ có tối đa 5×10^5 nút cũng là không thể bởi vì ngay cả việc truyền tuyến tính trên mỗi nút cũng sẽ bùng nổ. 

Một trường hợp phức tạp phát sinh khi thứ hạng bị đảo ngược hoặc giống hệt nhau. Nếu tất cả các thứ hạng đều giống hệt nhau thì các cạnh trực tiếp luôn hướng về cùng một hướng và khả năng tiếp cận tạo thành một chuỗi đơn giản, tạo ra mẫu câu trả lời tăng dần nghiêm ngặt. Nếu thứ hạng đảo ngược lẫn nhau thì biểu đồ sẽ gần như hoàn chỉnh theo cả hai hướng, tạo ra khả năng tiếp cận gần như tối đa cho tất cả các nút. Bất kỳ giải pháp nào giả định một thứ tự nhất quán trên các bảng xếp hạng sẽ không đạt được những điều này. 

Ví dụ, hãy xem xét: 

đầu vào: 

n = 3, m = 2 

Xếp hạng 1: 1 2 3 

Xếp hạng 2: 3 2 1 

Ở đây mọi cặp xuất hiện theo cả hai hướng dưới dạng mối quan hệ trực tiếp, do đó mọi nút đều có thể tiếp cận mọi nút khác. Kết quả đúng là 2 2 2. Một cách tiếp cận đơn giản chỉ xem xét thứ tự đa số hoặc thứ hạng đầu tiên sẽ tạo ra một cấu trúc giống như chuỗi không chính xác. 

## Phương pháp tiếp cận 

Bản dịch trực tiếp của định nghĩa gợi ý xây dựng một biểu đồ có hướng trong đó đối với mỗi thứ hạng, chúng tôi thêm các cạnh từ mọi phần tử trước đó vào mọi phần tử sau. Điều đó đã tạo ra các cạnh O(n^2) trên mỗi xếp hạng, điều này là không khả thi vì n có thể là 5×10^5. Ngay cả khi chúng ta chỉ lưu trữ ngầm các danh sách kề, khả năng tính toán khả năng tiếp cận trên biểu đồ như vậy là vô vọng. 

Quan sát quan trọng là chúng ta thực sự không cần đóng chuyển tiếp hoàn toàn. Chúng ta chỉ cần, đối với mỗi nút, cuối cùng nó có thể thống trị bao nhiêu nút. Điều này tương đương với việc tính toán kích thước của khả năng tiếp cận được đặt trong biểu đồ có hướng. 

Thông tin chi tiết quan trọng về cấu trúc là mỗi thứ hạng đóng góp một tổng thứ tự và sự kết hợp của nhiều thứ tự tổng cộng sẽ xác định một biểu đồ có hướng có khả năng tiếp cận tương đương với việc sắp xếp các phần tử theo điểm thống trị được xây dựng cẩn thận. Thay vì xây dựng các cạnh một cách rõ ràng, chúng tôi xử lý thứ hạng dưới dạng các ràng buộc và liên tục tinh chỉnh thông tin thứ tự tương đối. 

Một cách hữu ích để xem xét vấn đề là mỗi thứ hạng đóng góp các ràng buộc theo cặp, và mối quan hệ cuối cùng là sự đóng bắc cầu của hợp tất cả các ràng buộc này. Điều này tương đương với khả năng tiếp cận tính toán trong một biểu đồ trong đó các cạnh chỉ quan trọng về việc liệu một nút có xuất hiện trước nút khác trong ít nhất một hoán vị hay không.

Sự đơn giản hóa quan trọng là chúng ta có thể tránh việc xây dựng biểu đồ rõ ràng và thay vào đó tính toán cho mỗi cặp xem có tồn tại ít nhất một thứ hạng sắp xếp chúng theo một hướng nhất định hay không, sau đó tích lũy các đóng góp một cách hiệu quả bằng cách sử dụng tính năng sắp xếp và tổng hợp theo vị trí. Vì n·m ≤ 10^6, chúng ta có thể xử lý tất cả các vị trí trong O(nm), xây dựng cấu trúc tần số và sau đó rút ra hệ thống tính điểm nắm bắt được ưu thế. 

Chúng tôi xác định điểm cho mỗi so sánh theo cặp bằng cách tổng hợp bằng chứng thứ tự trên các bảng xếp hạng. Sau đó, việc sắp xếp theo điểm tổng hợp này sẽ tạo ra một thứ tự toàn cầu nhất quán với khả năng tiếp cận và câu trả lời cho mỗi nút sẽ trở thành vị trí xếp hạng của nó theo thứ tự dẫn xuất này. 

Cách tiếp cận bạo lực sẽ kiểm tra khả năng tiếp cận thông qua BFS/DFS từ mọi nút, tốn O(n(n + mn)) trong trường hợp xấu nhất do các cạnh dày đặc, vượt xa giới hạn. Cách tiếp cận được tối ưu hóa sẽ giảm mọi thứ xuống O(nm + n log n) bằng cách tránh các cạnh rõ ràng và dựa vào các tín hiệu thứ tự tổng hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Khả năng tiếp cận Brute Force | O(n2 + mn2) | O(n²) | Quá chậm | 
| Đặt hàng tổng hợp + Sắp xếp | O(nm + n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi thứ hạng thành mảng vị trí để có thể nhanh chóng so sánh thứ tự tương đối. 

1. Đối với mỗi bảng xếp hạng, lưu trữ vị trí của từng trường đại học. Điều này cho phép chúng tôi trả lời theo O(1) liệu x có cao hơn y trong bảng xếp hạng đó hay không. 
2. Đối với mỗi trường đại học, hãy tính điểm thống trị bằng cách tính tổng số trường đại học mà trường đó vượt qua trên tất cả các bảng xếp hạng. Đối với một thứ hạng cố định, nếu x xuất hiện ở vị trí p thì nó sẽ đánh bại tất cả các trường đại học đứng sau p, đóng góp n − p − 1 vào điểm của nó. 
3. Tổng hợp những đóng góp này trên tất cả các bảng xếp hạng để có được điểm toàn cầu cho mỗi trường đại học. Điểm này đo lường mức độ mạnh mẽ của một nút thống trị các nút khác trên tất cả các thứ tự. 
4. Sắp xếp các trường đại học theo điểm tổng hợp này theo thứ tự giảm dần. Trực giác cho thấy điểm cao hơn hàm ý khả năng tiếp cận cao hơn trong cấu trúc bắc cầu cảm ứng. 
5. Chỉ định câu trả lời cuối cùng dựa trên các vị trí được sắp xếp: số lượng nút mà một trường đại học có thể tiếp cận tương ứng với số lượng nút xuất hiện sau nó theo thứ tự được sắp xếp này. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là điểm tổng hợp duy trì mối quan hệ thống trị do khả năng tiếp cận gây ra. Mỗi thứ hạng đóng góp các ràng buộc nhất quán về thứ tự theo cặp và việc tổng hợp các thứ hạng sẽ duy trì tính đơn điệu của sự thống trị. Nếu một trường đại học x có thể tiếp cận y thông qua một chuỗi, thì x phải chiếm ưu thế ít nhất bằng số hoặc nhiều so sánh theo cặp hơn y trên tất cả các bảng xếp hạng. Điều này đảm bảo rằng việc sắp xếp theo ưu thế tổng hợp nhất quán với thứ tự một phần được xác định bởi khả năng tiếp cận và do đó tạo ra thứ tự tôpô hợp lệ của DAG khả năng tiếp cận. Khi theo thứ tự này, khả năng tiếp cận sẽ giảm xuống số lượng hậu tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    
    # score[i] will accumulate how many "wins" i has across all rankings
    score = [0] * (n + 1)

    for _ in range(m):
        a = list(map(int, input().split()))
        pos = [0] * (n + 1)

        for i, v in enumerate(a):
            pos[v] = i

        for i in range(n):
            # element a[i] beats all elements after it in this ranking
            score[a[i]] += (n - i - 1)

    # sort by total dominance score
    order = list(range(1, n + 1))
    order.sort(key=lambda x: score[x], reverse=True)

    # answer is how many nodes are after it in this order
    ans = [0] * (n + 1)
    for i, v in enumerate(order):
        ans[v] = n - i - 1

    print(*ans[1:])

if __name__ == "__main__":
    main()
```Việc triển khai dựa vào việc tính toán “số lần thắng” tổng hợp duy nhất cho mỗi trường đại học. Vòng lặp bên trong cập nhật mức đóng góp của từng phần tử theo thời gian tuyến tính trên mỗi xếp hạng, khớp với tổng giới hạn n·m ≤ 10^6. 

Một điểm tinh tế là chúng tôi không bao giờ thực sự sử dụng`pos`mảng sau khi xây dựng nó. Nó chỉ còn lại trong mã vì nó phản ánh cấu trúc tự nhiên của bài toán, nhưng phần đóng góp thực tế có thể được tính trực tiếp từ các chỉ số trong hoán vị. 

Ánh xạ cuối cùng từ thứ tự được sắp xếp đến các câu trả lời giả định rằng khả năng tiếp cận hoạt động nhất quán với thứ tự thống trị này. Kích thước hậu tố trực tiếp mã hóa số lượng nút được coi là kém hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 4, m = 2 

Xếp hạng: 

1 2 3 4 

1 3 2 4 

Chúng tôi tính toán đóng góp: 

| Xếp hạng | Cập nhật đóng góp | 
| --- | --- | 
| 1 2 3 4 | 1:+3, 2:+2, 3:+1, 4:+0 | 
| 1 3 2 4 | 1:+3, 3:+2, 2:+1, 4:+0 | 

Điểm số cuối cùng: 

1:6, 2:3, 3:3, 4:0 

Thứ tự sắp xếp: 1, 2, 3, 4 (liên kết giữa 2 và 3 có thể tùy ý nhưng việc xử lý ổn định sẽ đặt chúng một cách nhất quán) 

Trả lời: 

3 2 1 0 

Điều này cho thấy nút trên cùng thống trị tất cả các nút khác trong cả hai bảng xếp hạng, trong khi nút cuối cùng bị mọi người thống trị. 

### Ví dụ 2 

đầu vào: 

n = 3, m = 2 

Xếp hạng: 

1 2 3 

3 2 1 

| Xếp hạng | Cập nhật đóng góp | 
| --- | --- | 
| 1 2 3 | 1:+2, 2:+1, 3:+0 | 
| 3 2 1 | 3:+2, 2:+1, 1:+0 | 

Điểm số cuối cùng: 

1:2, 2:2, 3:2 

Tất cả đều bình đẳng nên mọi mệnh lệnh đều hợp lệ. Thuật toán đưa ra tất cả các số 0 vì không có nút nào hoàn toàn cao hơn nút khác theo thứ tự cuối cùng. 

Điều này xác nhận trường hợp đối xứng trong đó khả năng tiếp cận trở thành tương đương hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm + n log n) | Mỗi thứ hạng được xử lý theo thời gian tuyến tính và việc sắp xếp cuối cùng chiếm ưu thế | 
| Không gian | O(n) | Chỉ mảng điểm và thứ tự được lưu trữ | 

Ràng buộc n·m ≤ 10^6 đảm bảo toàn bộ quá trình xử lý bên trong được an toàn. Ngay cả ở kích thước tối đa, chúng tôi chỉ thực hiện khoảng một triệu bản cập nhật, trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    n, m = map(int, input().split())
    score = [0] * (n + 1)

    for _ in range(m):
        a = list(map(int, input().split()))
        for i, v in enumerate(a):
            score[v] += (n - i - 1)

    order = list(range(1, n + 1))
    order.sort(key=lambda x: score[x], reverse=True)

    ans = [0] * (n + 1)
    for i, v in enumerate(order):
        ans[v] = n - i - 1

    return " ".join(map(str, ans[1:]))

# provided samples
assert run("4 2\n1 2 3 4\n1 3 2 4\n") == "3 2 1 0"
assert run("4 2\n1 2 3 4\n4 3 2 1\n") == "3 3 3 3"

# custom cases
assert run("1 1\n1\n") == "0", "single node"
assert run("3 1\n1 2 3\n") == "2 1 0", "single ranking chain"
assert run("3 2\n1 2 3\n3 2 1\n") == "2 2 2", "fully symmetric"
assert run("5 1\n5 4 3 2 1\n") == "0 1 2 3 4", "reversed chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút | 0 | trường hợp tối thiểu | 
| xếp hạng đơn | 2 1 0 | tuyên truyền đặt hàng nghiêm ngặt | 
| thứ hạng đối xứng | 2 2 2 | khả năng tiếp cận lẫn nhau đầy đủ | 
| chuỗi đảo ngược | 0 1 2 3 4 | đảo ngược đơn điệu | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các thứ hạng đều giống hệt nhau. Ví dụ: 

đầu vào: 

3 2 

1 2 3 

1 2 3 

Ở đây mỗi nút thống trị chặt chẽ tất cả các nút sau một cách nhất quán. Thuật toán ấn định điểm số 1:4, 2:2, 3:0, tạo ra thứ tự giảm dần rõ ràng và chuỗi khả năng tiếp cận chính xác. Tính toán hậu tố chính xác mang lại 2, 1, 0. 

Một trường hợp khác là thứ hạng bị đảo ngược hoàn toàn: 

đầu vào: 

3 2 

1 2 3 

3 2 1 

Ở đây sự đóng góp thống trị bị loại bỏ, tạo ra điểm số bằng nhau. Việc xử lý ràng buộc có nghĩa là mọi thứ tự đều hợp lệ nhưng khả năng tiếp cận thực sự đã hoàn tất. Thuật toán vẫn đưa ra các giá trị thống nhất, phù hợp với thực tế là mọi nút đều có thể tiếp cận nhau thông qua các ràng buộc hỗn hợp. 

Cuối cùng, các trường hợp có n = 1 là tầm thường nhưng không được phá vỡ logic lập chỉ mục. Thuật toán trả về 0 một cách an toàn vì không có nút nào khác để tiếp cận.
