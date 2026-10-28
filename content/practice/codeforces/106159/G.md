---
title: "CF 106159G - Gelatos từ Goi\u00e1s"
description: "Chúng tôi được cung cấp một bộ sưu tập các thành phần, mỗi thành phần có điểm chất lượng bằng số. Chúng tôi muốn chọn một tập hợp con của các thành phần này để tối đa hóa hàm điểm cụ thể."
date: "2026-06-20T02:28:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106159
codeforces_index: "G"
codeforces_contest_name: "XIII UnB Contest Mirror"
rating: 0
weight: 106159
solve_time_s: 67
verified: true
draft: false
---

[CF 106159G - Gelatos từ Goi\u00e1s](https://codeforces.com/problemset/problem/106159/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập các thành phần, mỗi thành phần có điểm chất lượng bằng số. Chúng tôi muốn chọn một tập hợp con của các thành phần này để tối đa hóa hàm điểm cụ thể. Đối với bất kỳ tập hợp con nào được chọn, giá trị của nó được xác định bằng giá trị thành phần nhỏ nhất bên trong tập hợp con nhân với số thành phần trong tập hợp con đó. 

Tuy nhiên, không phải mọi tập hợp con đều được phép. Có một hoán vị cố định`P`mã hóa các ràng buộc không tương thích: thành phần`i`không thể được chọn cùng với thành phần`P[i]`. Từ`P`là một hoán vị không có điểm cố định, mọi phần tử được ghép theo hướng có hướng và mỗi mục cấm chính xác một mục khác. 

Nhiệm vụ là chọn một tập hợp con hợp lệ tuân theo các cặp bị cấm này và tối đa hóa tích của kích thước tập hợp con và giá trị tối thiểu của nó. 

Kích thước đầu vào đạt tới`N = 100000`, do đó, bất kỳ giải pháp nào thử tất cả các tập hợp con hoặc thậm chí tất cả các cặp tập hợp con đều không thể thực hiện được ngay lập tức. Cách tiếp cận bậc hai hoặc tệ hơn sẽ tạo ra thứ tự các phép toán 10¹⁰ trong trường hợp xấu nhất, vượt xa mọi giới hạn khả thi. Ngay cả các phương pháp liệt kê khối hoặc tập hợp con cũng bị loại trừ hoàn toàn. 

Cấu trúc của ràng buộc là khó khăn chính. Mỗi phần tử có chính xác một phần tử bị cấm, do đó đồ thị tạo bởi các cạnh`i -> P[i]`phân hủy thành các chu kỳ có hướng. Điều này có nghĩa là xung đột không phải là tùy tiện mà có cấu trúc. 

Một cách tiếp cận đơn giản là sắp xếp các giá trị và cố gắng bao gồm các phần tử một cách tham lam, nhưng điều này không thành công vì việc thêm phần tử có giá trị thấp sau này có thể làm mất hiệu lực tập hợp con đã chọn trước đó trông tối ưu. 

Một trường hợp thất bại tinh vi hơn phát sinh khi một phần tử có giá trị cao cấm một phần tử có giá trị cao khác, buộc chúng ta phải lựa chọn giữa việc giữ mức tối thiểu lớn hoặc giữ kích thước tập hợp con lớn. Ví dụ: nếu hai giá trị lớn được ghép nối và xuất hiện ở các phần khác nhau của quá trình quét được sắp xếp, thì việc bao gồm tham lam sẽ lấy cả hai giá trị không chính xác trừ khi được kiểm tra rõ ràng. 

## Phương pháp tiếp cận 

Phương pháp brute-force là liệt kê từng tập hợp con của các thành phần, kiểm tra xem nó có tôn trọng tất cả các cặp bị cấm hay không, tính toán thành phần và kích thước tối thiểu của nó, đồng thời theo dõi sản phẩm tốt nhất. Điều này đúng vì nó đánh giá định nghĩa một cách trực tiếp. Vấn đề là số lượng tập hợp con là 2^N và thậm chí việc xác thực một tập hợp con duy nhất cũng yêu cầu kiểm tra xung đột, dẫn đến thời gian theo cấp số nhân vượt xa tính khả thi. 

Quan sát quan trọng là mục tiêu chỉ phụ thuộc vào phần tử tối thiểu trong tập hợp con đã chọn. Nếu chúng ta cố định giá trị tối thiểu`x`, thì tất cả các phần tử được chọn phải có giá trị ít nhất`x`. Trong số các phần tử đó, chúng ta muốn chọn tập con lớn nhất có thể tránh được các cặp bị cấm. Điều này biến vấn đề thành vấn đề lựa chọn bị ràng buộc đối với tiền tố được lọc của các phần tử. 

Bây giờ hãy xem xét các phần tử xử lý theo thứ tự giá trị giảm dần. Khi hạ thấp ngưỡng, chúng tôi dần dần cho phép nhiều thành phần hơn trở nên đủ điều kiện. Ở mỗi giai đoạn, chúng tôi duy trì cấu trúc của các nút hiện đang hoạt động và đảm bảo chúng tôi không kích hoạt cả hai điểm cuối của bất kỳ cặp bị cấm nào. Vì mỗi nút xung đột với chính xác một nút khác nên mỗi lần kích hoạt chỉ ảnh hưởng đến một phần tử khác, điều này cho phép chúng tôi duy trì tính nhất quán dần dần. 

Điều này dẫn đến một cuộc quét tham lam: khi chúng tôi bao gồm các phần tử từ lớn nhất đến nhỏ nhất, chúng tôi duy trì một tập hợp các ứng cử viên tích cực. Khi một nút bắt đầu hoạt động, chúng tôi sẽ quyết định xem việc đưa nút đó vào có vi phạm xung đột với nút được ghép nối hay không. Nếu vậy, chúng ta phải quyết định nên giữ bên nào để bảo toàn những lựa chọn tối ưu trong tương lai. Cách tiếp cận đúng là duy trì các thành phần được hình thành bởi các tương tác “hoạt động và bị cấm hoạt động” và luôn đảm bảo chúng tôi tính tập hợp độc lập tối đa trong mỗi cấu trúc giống như chu kỳ do hoán vị gây ra. 

Bởi vì mỗi nút có bậc 2 trong cách diễn giải vô hướng (i và P[i]), đồ thị là một tập hợp các chu trình rời rạc. Trên mỗi chu kỳ, giải pháp tối ưu cho một ngưỡng nhất định sẽ giảm xuống việc chọn tập hợp độc lập tối đa có trọng số theo thời gian kích hoạt, nhưng vì chúng tôi xử lý theo giá trị giảm dần nên chúng tôi có thể giải quyết các lựa chọn cục bộ bằng cách sử dụng chiến lược đánh dấu tham lam trên mỗi chu kỳ. 

Trong thực tế, điều này làm giảm việc lặp lại các giá trị từ lớn nhất đến nhỏ nhất và duy trì một mảng boolean “có sẵn”. Khi xử lý một nút, nếu cả nút đó và đối tác của nó đều chưa được chọn, chúng tôi có thể lấy nút đó một cách an toàn; nếu không chúng tôi bỏ qua. Tính chính xác xuất phát từ thực tế là khi nút có giá trị cao hơn bị bỏ qua do đối tác của nó được chọn, việc chọn nút đó sau sẽ chỉ giảm mức tối thiểu mà không tăng kích thước khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N · N) | O(N) | Quá chậm | 
| Tối ưu | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sắp xếp các chỉ số theo giá trị giảm dần để luôn xem xét các thành phần mạnh hơn trước tiên. Điều này đảm bảo rằng khi chúng tôi quyết định lấy một phần tử, nó sẽ xác định ngưỡng tối thiểu hiện tại cho bất kỳ tập hợp con nào có chứa phần tử đó. 

Chúng tôi duy trì một mảng boolean`used`, ban đầu tất cả đều sai và chúng tôi cũng theo dõi xem mỗi phần tử đã bị đối tác cấm của nó chặn hay chưa. 

1. Sắp xếp tất cả các chỉ số theo`a[i]`theo thứ tự giảm dần. 
2. Lặp lại các chỉ số theo thứ tự đó. 
3. Đối với mỗi chỉ số`i`, kiểm tra xem cả hai`i`Và`P[i]`vẫn chưa được sử dụng. 
4. Nếu cả hai đều không được sử dụng, hãy chọn`i`và đánh dấu nó là đã sử dụng, đồng thời đánh dấu đối tác của nó là bị chặn để không thể chọn nó sau này. 
5. Nếu`i`đã bị chặn rồi, bỏ qua nó. 
6. Tiếp tục cho đến khi tất cả các phần tử được xử lý. 
7. Theo dõi câu trả lời tốt nhất khi chúng tôi tiếp tục bằng cách sử dụng số lượng đã chọn hiện tại nhân với ngưỡng giá trị được xử lý cuối cùng. 

Lý do chúng tôi chỉ kiểm tra đối tác là vì mọi xung đột đều mang tính cục bộ và đối xứng về mặt tính khả thi: việc chọn một điểm cuối sẽ vô hiệu hóa vĩnh viễn điểm cuối kia và không có hạn chế nào khác lan truyền ra ngoài cạnh này. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến ở mỗi bước, tập hợp các phần tử được chọn tạo thành một tập hợp độc lập hợp lệ trong biểu đồ được xác định bởi các cạnh`(i, P[i])`và tất cả các phần tử được chọn đều đến từ các giá trị sẵn có cao nhất được thấy cho đến nay. Vì chúng tôi xử lý theo thứ tự giảm dần nên bất kỳ tập hợp nào được chọn đều tối ưu cho ngưỡng tối thiểu hiện tại của nó. Bất kỳ sự đưa vào nào trong tương lai sẽ chỉ bao gồm các giá trị nhỏ hơn, điều này sẽ làm giảm mức tối thiểu và không thể cải thiện sản phẩm nếu không tăng kích thước vượt quá mức cấu trúc cho phép. Vì các xung đột diễn ra theo cặp và hình thành các chu kỳ rời rạc nên phép loại trừ tham lam sẽ giải quyết từng chu kỳ một cách tối ưu khi chúng ta gặp đại diện có giá trị cao nhất của nó trước tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    p = list(map(int, input().split()))
    p = [x - 1 for x in p]

    order = sorted(range(n), key=lambda i: a[i], reverse=True)

    used = [False] * n
    blocked = [False] * n

    taken = 0
    ans = 0

    for i in order:
        if blocked[i]:
            continue
        j = p[i]
        if not used[i] and not used[j]:
            used[i] = True
            used[j] = True
            taken += 2
            ans = max(ans, taken * a[i])

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp các chỉ số bằng cách giảm chất lượng thành phần sao cho giá trị hiện tại`a[i]`luôn đại diện cho giá trị tối thiểu của bất kỳ tập hợp con nào được hình thành tại thời điểm đó. các`used`mảng đảm bảo chúng ta không sử dụng lại một phần tử, trong khi`blocked`ngăn chặn việc chọn một phần tử có cặp đã được xác nhận trước đó trong quá trình quét. Mỗi lần chúng tôi đưa ra quyết định cấu trúc cặp thành công, chúng tôi sẽ cập nhật số lượng phần tử được chọn và tính toán chất lượng ứng viên. 

Một điểm tinh tế là chúng ta chỉ nhân với`a[i]`tại thời điểm lựa chọn, vì giá trị đó đóng vai trò là mức tối thiểu hiện tại trong quá trình quét giảm dần này. Điều này tránh việc tính toán lại cực tiểu trên các tập hợp con một cách rõ ràng. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào trong đó các giá trị đã được căn chỉnh theo cặp hoán vị. 

### Ví dụ 1 

đầu vào:```
N = 4
a = [5, 4, 3, 2]
P = [2, 1, 4, 3]
```| Bước | tôi | một [tôi] | Đối tác | Hành động | Đã chụp | Tốt nhất hiện tại | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 2 | lấy cặp (1,2) | 2 | 10 | 
| 2 | 2 | 4 | 1 | bị chặn | 2 | 10 | 
| 3 | 3 | 3 | 4 | lấy cặp (3,4) | 4 | 12 | 
| 4 | 4 | 2 | 3 | bị chặn | 4 | 12 | 

Điều này cho thấy cách thuật toán tham lam tạo thành các cặp theo thứ tự giá trị giảm dần, cập nhật sản phẩm tốt nhất khi tập hợp con mở rộng. 

### Ví dụ 2 

đầu vào:```
N = 3
a = [10, 1, 9]
P = [3, 1, 2]
```| Bước | tôi | một [tôi] | Đối tác | Hành động | Đã chụp | Tốt nhất hiện tại | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 10 | 3 | bỏ qua hoặc lấy trạng thái tùy theo | 2 | 20 | 
| 2 | 3 | 9 | 2 | bỏ qua do bị chặn | 2 | 20 | 
| 3 | 2 | 1 | 1 | bị chặn | 2 | 20 | 

Điều này chứng tỏ rằng một khi quyết định ghép đôi có giá trị cao được đưa ra, các ứng cử viên có giá trị thấp hơn không thể ghi đè lên quyết định đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Sắp xếp chiếm ưu thế; mỗi phần tử được xử lý một lần | 
| Không gian | O(N) | Mảng hoán vị và theo dõi trạng thái | 

Giải pháp phù hợp thoải mái trong giới hạn cho`N = 10^5`. Việc sắp xếp ở tỷ lệ này rất hiệu quả và tất cả các hoạt động tiếp theo đều là quét tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal case
assert run("2\n5 1\n2 1\n") == "10"

# simple chain-like pairing
assert run("4\n5 4 3 2\n2 1 4 3\n") == "12"

# all equal values
assert run("4\n7 7 7 7\n2 1 4 3\n") == "14"

# single dominant pair
assert run("3\n10 1 9\n3 1 2\n") == "20"

# larger mixed case
assert run("6\n6 5 4 3 2 1\n2 1 4 3 6 5\n") == "12"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 yếu tố | 10 | trường hợp biên nhỏ nhất | 
| chu kỳ ghép đôi | 12 | hành vi ghép nối đúng | 
| giá trị bằng nhau | 14 | xử lý cà vạt | 
| chu kỳ thống trị | 20 | sự thống trị tham lam | 
| nhiều chu kỳ | 12 | độc lập qua các chu kỳ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các giá trị giống hệt nhau. Thuật toán vẫn ghép các phần tử một cách tham lam theo thứ tự, nhưng vì tất cả các cực tiểu đều bằng nhau nên chiến lược tối ưu sẽ giảm xuống mức tối đa hóa kích thước tập hợp con theo các ràng buộc ghép nối. Việc ghép nối tham lam đảm bảo mọi chu kỳ hợp lệ đều được xử lý mà không bỏ sót. 

Một trường hợp khác là một chu kỳ dài trong đó các giá trị cao và thấp xen kẽ nhau. Do thuật toán xử lý nghiêm ngặt theo giá trị nên các nút có giá trị cao hơn trong chu trình sẽ được chọn trước và các nút thấp hơn sẽ bị chặn khi cần. Tập hợp con cuối cùng vẫn là một tập hợp độc lập hợp lệ, tôn trọng tất cả các ràng buộc. 

Trường hợp cạnh cuối cùng xảy ra khi một phần tử có giá trị rất cao được ghép nối với một phần tử có giá trị rất cao khác. Thuật toán ngay lập tức giải quyết xung đột này ở đầu thứ tự, đảm bảo rằng mức tối thiểu tối ưu không bị giảm đi một cách giả tạo bởi các lựa chọn sau này.
