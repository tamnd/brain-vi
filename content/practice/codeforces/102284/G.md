---
title: "CF 102284G - SIS"
description: "Chúng tôi nhận được từng công tắc một. Công tắc i có nhãn số nguyên a[i] và nếu hai công tắc có nhãn x và y được kết nối thì kênh đó có giá x XOR y."
date: "2026-08-14T04:24:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102284
codeforces_index: "G"
codeforces_contest_name: "\u041b\u041a\u0428 2019, \u0418\u044e\u043b\u044c, \u041c\u0438\u043a\u0441 \u0441\u0442\u0430\u0440\u0448\u0435\u0439 \u0438 \u043c\u043b\u0430\u0434\u0448\u0435\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434"
rating: 0
weight: 102284
solve_time_s: 1368
verified: false
draft: false
---

[CF 102284G - SIS](https://codeforces.com/problemset/problem/102284/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 22 phút 48 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được từng công tắc một. Công tắc`i`có nhãn số nguyên`a[i]`và nếu hai công tắc có nhãn`x`Và`y`được kết nối, chi phí của kênh đó`x XOR y`. Đối với mỗi tiền tố của chuỗi đầu vào, chúng ta cần tổng chi phí tối thiểu có thể có của một mạng được kết nối chứa chính xác các bộ chuyển mạch đã đến. 

Vì bất kỳ mạng được kết nối nào cũng có thể được giảm xuống thành cây bao trùm mà không làm tăng chi phí của nó, nên nhiệm vụ sau mỗi lần chèn chính xác là trọng số cây bao trùm tối thiểu của đồ thị hoàn chỉnh có các đỉnh là các công tắc hiện tại và có trọng số cạnh nằm giữa`x`Và`y`là`x XOR y`. 

Số lượng công tắc cuối cùng có thể là`100000`, vì vậy việc xây dựng lại MST từ đầu cho mọi tiền tố là quá tốn kém. Ngay cả việc triển khai Prim bậc hai trên tiền tố kích thước`k`cần Θ(`k²`) so sánh cạnh. Tổng hợp tất cả các tiền tố, đây là Θ(`n³`), Về`3.3 * 10^14`so sánh khi`n = 100000`. Thuật toán bậc hai hoặc bậc ba không thể phù hợp với giới hạn thời gian lập trình cạnh tranh thông thường. 

Các giá trị thỏa mãn`0 <= a[i] <= 200000`. Từ`200000 < 2^18`, mọi giá trị chỉ cần 18 bit nhị phân, từ bit 17 xuống bit 0. Độ rộng bit nhỏ này là thuộc tính cấu trúc làm cho trie nhị phân trở nên hữu ích. Một đa thức thuật toán về số lượng bit, thay vì số lượng các cặp chuyển mạch có thể có, là khả thi. 

Có một số trường hợp khó xử lý. Đầu tiên, các giá trị bằng nhau là các công tắc riêng biệt nhưng chi phí kết nối của chúng bằng 0. Đối với đầu vào`3`với các giá trị`5 5 5`, câu trả lời là`0 0 0`. Việc triển khai xử lý các giá trị bằng nhau dưới dạng các đỉnh trùng lặp vẫn có thể nhận được trọng số MST ngay tại đây, nhưng nó có thể dễ dàng phá vỡ logic chèn của nó bằng cách giả sử mỗi giá trị mới sẽ tạo ra một lá trie mới. 

Một trường hợp tinh tế hơn là khi thêm một switch sẽ thay đổi cạnh tốt nhất giữa hai cây con trie nhị phân, đồng thời thay đổi MST bên trong một trong những cây con đó. Ví dụ, với`0 7 3`, câu trả lời là`0 7 7`. Sau khi chèn`3`, cạnh tốt nhất giữa`{0,3}`Và`{7}`trở thành`3 XOR 7 = 4`, thay thế cạnh chéo trước đó của chi phí`7`, nhưng cây con`{0,3}`bản thân nó bây giờ cần một lợi thế về chi phí`3`. Hai thay đổi hủy bỏ. Một giải pháp bất cẩn chỉ cần thêm khoảng cách gần nhất của công tắc mới vào MST trước đó sẽ thu được`7 + 3 = 10`, điều đó là sai. 

Bit cao nhất được phép là một trường hợp ranh giới khác. Đối với đầu vào`3`với các giá trị`0 131072 131073`, câu trả lời là`0 131072 131073`. Từ`131072 = 2^17`, bit 17 phải được thêm vào. Chỉ sử dụng các bit từ 16 đến 0 sẽ âm thầm làm mất đi sự đóng góp đáng kể nhất và tạo ra kết quả không chính xác. 

Cuối cùng, công tắc đầu tiên không có cạnh nào cả nên chi phí MST của nó bằng 0. Vì`2`với các giá trị`0 1`, đầu ra cần thiết là`0 1`. Mã cập nhật câu trả lời trước khi có nhánh trie đối diện có thể vô tình đưa ra một cạnh không tồn tại. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý mọi tiền tố một cách độc lập. Đối với tiền tố có chứa`k`chuyển mạch, chúng ta có thể xây dựng đồ thị hoàn chỉnh một cách ngầm định và chạy thuật toán Prim. Đối với mỗi đỉnh mới được chọn, chúng tôi quét tất cả các đỉnh còn lại và tính khoảng cách XOR của chúng. Điều này đúng vì Prim luôn tạo MST và biểu đồ hoàn chỉnh chứa mọi kênh có thể. Vấn đề là công việc lặp đi lặp lại. Một tiền tố có giá Θ(`k²`), vì vậy tất cả các tiền tố đều có giá`1² + 2² + ... + n² = Θ(n³)`. 

Vì`n = 100000`, đại khái là vậy`3.33 * 10^14`kiểm tra theo cặp, điều này gần như không thực tế. 

Quan sát quan trọng xuất phát từ việc xem xét XOR theo bit khác biệt cao nhất của nó. Hãy xem xét tất cả các giá trị chia sẻ một số tiền tố cố định và giả sử bit liên quan tiếp theo của chúng là`b`. Chia chúng thành`L`, bit của ai`b`bằng không, và`R`, bit của ai`b`là một. Mỗi cạnh bên trong`L`và mọi cạnh bên trong`R`có chút`b`bằng không. Mỗi cạnh giữa`L`Và`R`có chút`b`bằng một. Vì tất cả các bit cao hơn đều bằng nhau bên trong nút trie này nên mọi cạnh bên trong đều rẻ hơn mọi cạnh chéo. 

Điều đó có nghĩa là Kruskal sẽ hoàn toàn kết nối`L`Và`R`nội bộ trước khi xem xét bất kỳ lợi thế nào giữa chúng. Sau khi cả hai bên được kết nối, cần có chính xác một cạnh chéo. Do đó, cạnh chéo rẻ nhất có thể là XOR tối thiểu giữa một giá trị trong`L`và một giá trị trong`R`. 

Điều này mang lại sự tái diễn cơ bản`MST(node) = MST(left) + MST(right) + minimum_cross(node)`, 

trong đó số hạng cuối cùng bằng 0 nếu một trong hai phần tử con trống. 

Trie nhị phân thể hiện chính xác các phân vùng đệ quy này. Vấn đề còn lại là động: sau khi chèn một giá trị, chỉ các nút trie trên đường dẫn từ gốc đến lá của giá trị đó mới có thể thay đổi. Tại mỗi nút như vậy, MST bên trong của nút con được chọn sẽ thay đổi sâu hơn trong trie và cạnh chéo mới duy nhất có thể là cạnh từ giá trị mới được chèn đến nút con đối diện. 

Vì vậy, chúng tôi duy trì, với mỗi nút trie, cạnh rẻ nhất hiện tại kết nối hai nút con của nó. Khi một giá trị mới`x`được chèn vào, chúng tôi đi theo con đường thực sự của nó. Tại một nút tương ứng với bit`b`, nếu cây con đối diện đã tồn tại, chúng ta truy vấn cây con đó để tìm giá trị cực tiểu`x XOR y`. Điều này mang lại ứng cử viên mới duy nhất có thể cải thiện cạnh chéo của nút. Chúng tôi cập nhật câu trả lời MST toàn cầu dựa trên sự khác biệt giữa chi phí chéo mới và cũ. 

Bản thân truy vấn XOR tối thiểu là bước đi tham lam nhị phân tiêu chuẩn. Tại mỗi bit thấp hơn, chúng ta ưu tiên nhánh chứa bit giống như`x`, bởi vì điều đó đóng góp bằng 0 cho XOR. Nếu nhánh đó không tồn tại, chúng ta lấy nhánh kia và cộng lũy ​​thừa tương ứng của 2 vào kết quả truy vấn. 

Brute-force hoạt động vì nó tính toán lại rõ ràng tất cả các mối quan hệ cặp. Nó thất bại vì hầu như tất cả thông tin đó không thay đổi giữa các tiền tố liên tiếp. Quan sát cho thấy MST phân rã dọc theo các tiền tố nhị phân cho phép chúng tôi chỉ cập nhật một đường dẫn từ gốc đến lá và mỗi truy vấn chéo là một bước đi khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n^3)`tổng cộng |`O(n)`| Quá chậm | 
| Tối ưu |`O(n log^2 A)`|`O(n log A)`| Đã chấp nhận | 

Đây`A`là giá trị lớn nhất có thể, và trong bài toán này`log A <= 18`. 

## Hướng dẫn thuật toán 

1. Xây dựng một trie nhị phân trống ban đầu chứa tất cả các giá trị được chèn cho đến nay. Mỗi nút trie lưu trữ hai nút con của nó và cạnh XOR tối thiểu hiện tại kết nối các nút con đó. 

Gốc đại diện cho tất cả các switch được chèn vào. Tiếp theo bit 17, rồi đến bit 16, v.v., phân vùng các công tắc theo tiền tố nhị phân của chúng. 
2. Đối với mỗi giá trị đến`x`, bắt đầu từ gốc và đi theo đường dẫn tương ứng với các bit của`x`từ bit 17 xuống bit 0. 

Chỉ các nút trên đường dẫn này mới có thể thay đổi khi`x`được thêm vào. Mọi nút trie khác đều chứa chính xác cùng một bộ công tắc cũ như trước. 
3. Tại nút tương ứng với bit`b`, xác định con chứa bit giống như`x`và đứa trẻ ngược lại. 

Nếu con đối diện không tồn tại thì trước khi chèn không có cạnh chéo và vẫn không có cạnh chéo sau khi chèn. Chúng ta chỉ tiếp tục xuống con phù hợp. 
4. Nếu cây con đối diện tồn tại, hãy truy vấn cây con đó để tìm XOR tối thiểu với`x`, chỉ xem xét các bit bên dưới`b`. 

Bit hiện tại đóng góp chính xác`2^b`, bởi vì hai đứa trẻ có chút khác biệt`b`. Truy vấn trie bit thấp hơn xác định phần còn lại của trọng số cạnh. 
5. So sánh trọng số cạnh chéo ứng cử viên này với giá trị được lưu trữ cho nút. 

Các cạnh mới duy nhất được đưa vào bằng cách chèn này là các cạnh từ`x`đến các switch cũ. Trong số các cạnh đi qua hai nút con của nút này, mọi cặp cũ đều đã tồn tại. Do đó mức tối thiểu cũ chỉ có thể không thay đổi hoặc được thay thế bằng cạnh từ`x`tới giá trị gần nhất ở con đối diện. 
6. Nếu ứng viên nhỏ hơn, hãy thay thế giá trị cạnh chéo được lưu trữ và thêm phần chênh lệch vào câu trả lời MST toàn cục. 

Việc giảm ở đây là có thể. Ví dụ: khi chèn`3`vào trong`{0,7}`, cạnh chéo của gốc cải thiện từ`7`ĐẾN`4`. Đồng thời, nút sâu hơn sẽ có được lợi thế bên trong`0 XOR 3 = 3`. Câu trả lời chung ghi lại cả hai thay đổi, đưa ra`7 - 3 + 3 = 7`. 
7. Tạo bit con phù hợp nếu nó chưa tồn tại, sau đó tiếp tục tới bit thấp hơn tiếp theo. 

Việc tạo đường dẫn trie diễn ra sau khi kiểm tra cây con đối diện, vì vậy mọi truy vấn chỉ xem xét các khóa chuyển có mặt trước khi chèn hiện tại. 
8. Sau khi xử lý tất cả 18 bit, hãy thêm câu trả lời chung hiện tại vào đầu ra. 

Đối với một công tắc không có cạnh nào, vì vậy câu trả lời đương nhiên vẫn là 0. 

### Tại sao nó hoạt động 

Đối với mỗi nút trie, tất cả các giá trị trong cây con của nó đều có cùng tiền tố nhị phân cao hơn. Nếu con của nó khác nhau một chút`b`, mọi cạnh đi qua hai con đều có mức đóng góp cao hơn như nhau và có bit`b`được thiết lập, trong khi mọi cạnh ở bên trong một phần tử con đều có bit`b`bỏ đặt. Do đó, tất cả các cạnh bên trong đều rẻ hơn mọi cạnh chéo. Kruskal phải hoàn thành hai bài toán con trước khi sử dụng cạnh chéo và chính xác một cạnh chéo tối thiểu là đủ để kết nối chúng. Điều này chứng tỏ sự phân rã MST đệ quy. 

Trong quá trình chèn, tất cả các nút bên ngoài đường trie của giá trị mới đều chứa các đỉnh giống hệt nhau, do đó chi phí MST và cực tiểu chéo của chúng không thể thay đổi. Tại một nút đường dẫn, các cạnh chéo mới có sẵn duy nhất là những cạnh liên quan đến giá trị mới. Truy vấn trie tìm cạnh rẻ nhất như vậy. Do đó, việc cập nhật mức tối thiểu chéo được lưu trữ là chính xác và tổng của tất cả các đóng góp của nút đã thay đổi chính xác là trọng số MST mới. Bằng cách quy nạp qua các lần chèn, mọi câu trả lời được báo cáo đều tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAX_BIT = 17

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # Each node has two children.
    # -1 means that the child does not exist.
    left = [-1]
    right = [-1]

    # cross[v] is the minimum XOR edge between the two
    # children of v. Zero means that one child is absent.
    cross = [0]

    answer = 0
    out = []

    for x in a:
        node = 0

        for b in range(MAX_BIT, -1, -1):
            bit = (x >> b) & 1

            if bit == 0:
                opposite = right[node]
            else:
                opposite = left[node]

            if opposite != -1:
                # Find the minimum XOR between x and a value
                # in the opposite subtree, using lower bits.
                cur = opposite
                lower_xor = 0
                k = b - 1

                while k >= 0:
                    xb = (x >> k) & 1

                    if xb == 0:
                        nxt = left[cur]
                        if nxt == -1:
                            nxt = right[cur]
                            lower_xor |= 1 << k
                    else:
                        nxt = right[cur]
                        if nxt == -1:
                            nxt = left[cur]
                            lower_xor |= 1 << k

                    cur = nxt
                    k -= 1

                candidate = (1 << b) + lower_xor
                old = cross[node]

                if old == 0 or candidate < old:
                    cross[node] = candidate
                    if old == 0:
                        answer += candidate
                    else:
                        answer += candidate - old

            # Move down the trie, creating the new path if necessary.
            if bit == 0:
                nxt = left[node]
                if nxt == -1:
                    nxt = len(left)
                    left.append(-1)
                    right.append(-1)
                    cross.append(0)
                    left[node] = nxt
            else:
                nxt = right[node]
                if nxt == -1:
                    nxt = len(left)
                    left.append(-1)
                    right.append(-1)
                    cross.append(0)
                    right[node] = nxt

            node = nxt

        out.append(str(answer))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Ba mảng đại diện cho trie nhị phân.`left[v]`Và`right[v]`xác định những đứa trẻ, trong khi`cross[v]`lưu trữ cạnh rẻ nhất nối hai cây con con. Không cần phải lưu trữ các đỉnh bên trong các nút trie. 

Vòng lặp kết thúc`b`theo giá trị mới từ bit có liên quan cao nhất đến bit thấp nhất. Bit có liên quan cao nhất là 17 vì`200000 < 2^18`. Sử dụng 18 bit là đủ cho mọi đầu vào hợp pháp, trong khi bắt đầu từ bit 18 cũng sẽ hoạt động nhưng sẽ thêm công việc không cần thiết. 

Trước khi chuyển sang thành phần con phù hợp, mã sẽ kiểm tra thành phần con đối diện. Thứ tự này quan trọng. Cây con đối diện chỉ được chứa các switch được chèn trước đó vì switch mới chưa được gắn vào trie. Nếu không thì truy vấn có thể vô tình sử dụng`x`với tư cách là đối tác của chính mình. 

Truy vấn bắt đầu trực tiếp từ con đối diện thay vì từ gốc. Tại bit hiện tại`b`, hai tiền tố con đã khác nhau nên sự đóng góp của bit đó được gọi là`2^b`. Chỉ những bit thấp hơn mới cần được tối ưu hóa. Đây là lý do tại sao truy vấn bắt đầu lúc`b - 1`. 

Khi nhánh trie ưa thích bị thiếu, truy vấn sẽ lấy nhánh khác và đặt bit tương ứng vào`lower_xor`. Điều này thực hiện chính xác tìm kiếm XOR tối thiểu: các bit phù hợp luôn được ưu tiên hơn vì chúng không đóng góp gì. 

các`cross`value sử dụng số 0 làm trọng điểm vì bất kỳ cạnh thực nào giữa hai phần tử con khác nhau đều có ít nhất một bit khác nhau, do đó giá trị XOR của nó là dương. Các nhãn chuyển đổi bằng nhau vẫn nằm trong cùng một lá trie và không bao giờ tạo ra cạnh chéo dương. 

Số nguyên Python tự động xử lý tổng MST có thể lớn, do đó không có vấn đề tràn. Câu trả lời tối đa có thể nằm trong khả năng số nguyên của Python trong mọi trường hợp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với trình tự`4 0 2 2 9 4`, những thay đổi quan trọng được hiển thị bên dưới. 

| Giá trị được chèn | Đã thay đổi cấp độ trie | Chữ thập cũ | Chữ thập mới | Đồng bằng | Tổng số câu trả lời | 
| --- | --- | --- | --- | --- | --- | 
|`4`| không | 0 | 0 |`0`|`0`| 
|`0`| bit 2 | 0 | 4 |`+4`|`4`| 
|`2`| bit 1 | 0 | 2 |`+2`|`6`| 
|`2`| không | không thay đổi | không thay đổi |`0`|`6`| 
|`9`| bit 3 | 0 | 9 |`+9`|`15`| 
|`4`| không | không thay đổi | không thay đổi |`0`|`15`| 

Sau khi chèn`0`, hai giá trị được phân tách lần đầu tiên ở bit 2, do đó cạnh duy nhất có giá trị`4 XOR 0 = 4`. 

Khi`2`đến, nó thuộc cùng phía bit-2 với`0`, nhưng khác với`0`tại bit 1. Điều đó tạo ra kết nối chi phí nội bộ rẻ hơn`2`. Tổng số trở thành`6`. 

thứ hai`2`là một giá trị trùng lặp. Nó tạo ra một công tắc khác nhưng mọi cạnh mới từ nó đến một công tắc hiện có đều có cùng chi phí với cạnh tương ứng từ cạnh đầu tiên.`2`và đặc biệt là nó có thể sử dụng lợi thế chi phí bằng 0 cho bộ chuyển mạch đó. Trọng lượng MST vẫn giữ nguyên`6`. 

giá trị`9`đầu tiên khác với các giá trị hiện có ở bit 3. Cạnh rẻ nhất nối mặt bit-3 mới với mặt cũ có chi phí`9`, tăng câu trả lời cho`15`. trận chung kết`4`đã được đại diện bởi giá trị hiện có`4`, do đó có sẵn kết nối không tốn phí và trọng lượng MST vẫn được duy trì`15`. 

### Ví dụ thứ hai 

Hãy xem xét trình tự`0 7 3 4`. 

| Giá trị được chèn | Đã thay đổi cấp độ | Chữ thập cũ | Chữ thập mới | Đồng bằng | Tổng số câu trả lời | 
| --- | --- | --- | --- | --- | --- | 
|`0`| không | 0 | 0 |`0`|`0`| 
|`7`| bit 2 | 0 | 7 |`+7`|`7`| 
|`3`| bit 2 | 7 | 4 |`-3`|`4`| 
|`3`| bit 1 | 0 | 3 |`+3`|`7`| 
|`4`| bit 1 | 0 | 3 |`+3`|`10`| 

Phần chèn thứ ba thể hiện phần tinh tế nhất của bản cập nhật. Trước khi chèn`3`, gốc tách ra`{0}`từ`{7}`, vậy chi phí chéo của nó là`7`. Sau khi chèn`3`, gốc tách ra`{0,3}`từ`{7}`, và cạnh chéo tốt nhất là`3 XOR 7 = 4`. Phần đóng góp gốc giảm đi bởi`3`. 

Đồng thời, cây con chứa`{0,3}`đạt được lợi thế chi phí MST của riêng mình`0 XOR 3 = 3`. Tổng số thay đổi là`-3 + 3 = 0`, vậy đáp án vẫn là`7`. 

Sau khi chèn`4`, cây con chứa`7`Và`4`đạt được lợi thế`4 XOR 7 = 3`. MST thu được có thể sử dụng các cạnh có chi phí`3`,`3`, Và`4`, với tổng số`10`. 

Dấu vết này đặc biệt hữu ích vì nó cho thấy tại sao chỉ duy trì khoảng cách từ công tắc mới đến MST cũ là không đủ. Một số cấp độ thử nghiệm có thể thay đổi cùng một lúc và mức đóng góp của chúng có thể tăng hoặc giảm một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log^2 A)`| có`O(log A)`ba cấp độ và ở mỗi cấp độ, một truy vấn XOR tối thiểu cần`O(log A)`| 
| Không gian |`O(n log A)`| Nhiều nhất`O(n log A)`các nút trie được tạo | 

Đây`A <= 200000`, vì vậy chỉ có 18 bit liên quan. Mỗi lần chèn thực hiện tối đa 18 tìm kiếm XOR tối thiểu và tổng số bước bit thấp hơn nhiều nhất là`17 + 16 + ... + 0`, chỉ có 153 bước thử cho mỗi lần chèn. Vì`100000`chuyển đổi, đây là khoảng 15,3 triệu bước truy vấn bên trong, cộng với công việc chèn tri thông thường. 

Việc sử dụng bộ nhớ cũng có thể quản lý được. Thực ra, vì chỉ có`2^18`giá trị có thể, trie nhị phân hoàn chỉnh sẽ chứa ít hơn`2^19`các nút, mặc dù chung`O(n log A)`ràng buộc là đủ cho việc phân tích. 

## Trường hợp thử nghiệm```python
# Helper: run the solution logic on an input string and return its output.
import sys
import io

MAX_BIT = 17

def solve_data(data: str) -> str:
    inp = io.StringIO(data)
    n = int(inp.readline())
    a = list(map(int, inp.readline().split()))

    left = [-1]
    right = [-1]
    cross = [0]

    answer = 0
    out = []

    for x in a:
        node = 0

        for b in range(MAX_BIT, -1, -1):
            bit = (x >> b) & 1

            if bit == 0:
                opposite = right[node]
            else:
                opposite = left[node]

            if opposite != -1:
                cur = opposite
                lower_xor = 0
                k = b - 1

                while k >= 0:
                    xb = (x >> k) & 1

                    if xb == 0:
                        nxt = left[cur]
                        if nxt == -1:
                            nxt = right[cur]
                            lower_xor |= 1 << k
                    else:
                        nxt = right[cur]
                        if nxt == -1:
                            nxt = left[cur]
                            lower_xor |= 1 << k

                    cur = nxt
                    k -= 1

                candidate = (1 << b) + lower_xor
                old = cross[node]

                if old == 0 or candidate < old:
                    cross[node] = candidate
                    answer += candidate if old == 0 else candidate - old

            if bit == 0:
                nxt = left[node]
                if nxt == -1:
                    nxt = len(left)
                    left.append(-1)
                    right.append(-1)
                    cross.append(0)
                    left[node] = nxt
            else:
                nxt = right[node]
                if nxt == -1:
                    nxt = len(left)
                    left.append(-1)
                    right.append(-1)
                    cross.append(0)
                    right[node] = nxt

            node = nxt

        out.append(str(answer))

    return "\n".join(out)

def run(inp: str) -> str:
    return solve_data(inp)

# Provided sample
assert run("6\n4 0 2 2 9 4\n") == "0\n4\n6\n6\n15\n15", "sample 1"

# Minimum-size input
assert run("2\n0 1\n") == "0\n1", "minimum size"

# All values equal
assert run("4\n123 123 123 123\n") == "0\n0\n0\n0", "all equal"

# Highest allowed bit and a lower-bit refinement
assert run("3\n0 131072 131073\n") == "0\n131072\n131073", "bit 17 boundary"

# Cases where the new vertex changes different trie levels
assert run("4\n0 7 3 4\n") == "0\n7\n7\n10", "multiple levels"

# Maximum-size input, all equal, answer always zero
vals = " ".join(["0"] * 100000)
expected = "\n".join(["0"] * 100000)
assert run("100000\n" + vals + "\n") == expected, "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 0 1`|`0 1`| Số lượng công tắc tối thiểu và cạnh khác 0 đầu tiên | 
|`4 / 123 123 123 123`|`0 0 0 0`| Giá trị trùng lặp và cạnh không tốn phí | 
|`3 / 0 131072 131073`|`0 131072 131073`| Bit cao nhất được phép, cộng với cải tiến bit thấp hơn | 
|`4 / 0 7 3 4`|`0 7 7 10`| Nhiều cấp độ thử thay đổi trong một lần chèn | 
|`100000`số không | 100000 số không | Kích thước đầu vào tối đa và các giá trị giống hệt nhau lặp lại | 

## Vỏ cạnh 

Trường hợp giá trị trùng lặp được tri xử lý một cách tự nhiên. Đối với đầu vào`4`với các giá trị`123 123 123 123`, mọi giá trị mới đều đi theo cùng một đường dẫn như giá trị đầu tiên. Không có con đối diện nào được tạo ở bất kỳ cấp độ nào, do đó không có thay đổi đóng góp chéo nào. Đầu ra là`0 0 0 0`. Các công tắc vẫn là các đỉnh riêng biệt, nhưng việc kết nối các nhãn bằng nhau không tốn phí. 

Lần chèn đầu tiên không có cây con đối diện ở bất kỳ đâu vì trie không chứa giá trị trước đó. Đối với đầu vào`2`với các giá trị`0 1`, câu trả lời đầu tiên là`0`. Khi`1`được chèn vào, hai giá trị đầu tiên khác nhau ở bit 0, truy vấn bên dưới bit đó trống và cạnh chéo mới có giá trị`1`. Đầu ra là`0 1`. 

Ranh giới giá trị tối đa sử dụng bit 17. Đối với đầu vào`3`với các giá trị`0 131072 131073`, chèn`131072`tạo ra sự phân chia ở bit 17 và thêm`2^17 = 131072`. Khi`131073`đến, nó thuộc cùng cây con bit-17 như`131072`và chỉ khác nó ở bit 0, do đó MST đạt được lợi thế về chi phí`1`. Đầu ra là`0 131072 131073`. 

Trường hợp lừa đảo nhất là`0 7 3`. Sau đó`0`Và`7`, cạnh duy nhất có chi phí`7`. Khi`3`đến, cạnh chéo tốt nhất của gốc cải thiện từ`7`ĐẾN`3 XOR 7 = 4`, giảm sự đóng góp gốc bằng cách`3`. Ở bit dưới nơi`0`Và`3`chia tách, một lợi thế nội bộ mới của chi phí`3`xuất hiện. Hai thay đổi bị hủy bỏ, để lại câu trả lời tại`7`. Đây chính xác là lý do tại sao thuật toán cập nhật mọi nút trie trên đường dẫn chèn thay vì coi công tắc mới là một cạnh được thêm vào MST trước đó. 

Mẫu cuối cùng cũng thực hiện thao tác chèn trùng lặp sau khi một số cấp độ tri thức đã được xây dựng. TRONG`4 0 2 2 9 4`, thứ hai`2`và thứ hai`4`không cải thiện bất kỳ cạnh chéo được lưu trữ nào, vì vậy câu trả lời không thay đổi ở các lần chèn tương ứng của chúng. Thuật toán không cần xử lý trùng lặp đặc biệt vì cấu trúc tri và các truy vấn XOR tối thiểu đã chiếm các kết nối không tốn phí.
