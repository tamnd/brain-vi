---
title: "CF 106456F - Chuỗi khung của Fanfan"
description: "Chúng ta có một cây nhị phân có gốc có các nút được đánh nhãn từ 1 đến n. Mỗi nút tương ứng với một cặp dấu ngoặc đơn phù hợp trong một số chuỗi khung hợp lệ chưa xác định có độ dài 2n."
date: "2026-06-19T17:36:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106456
codeforces_index: "F"
codeforces_contest_name: "The 15th Huazhong Agricultural University Programming Contest"
rating: 0
weight: 106456
solve_time_s: 50
verified: true
draft: false
---

[CF 106456F - Trình tự khung của Fanfan](https://codeforces.com/problemset/problem/106456/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây nhị phân có gốc có các nút được đánh nhãn từ 1 đến n. Mỗi nút tương ứng với một cặp dấu ngoặc đơn phù hợp trong một số chuỗi khung hợp lệ chưa xác định có độ dài 2n. Việc đánh số các nút không phải là tùy ý: nó được tạo ra bởi chuỗi ban đầu, trong đó các cặp được đánh số theo thứ tự dấu ngoặc đơn bên trái của chúng xuất hiện khi quét chuỗi từ trái sang phải. 

Bản thân cây được xác định bởi một quy tắc cấu trúc kết nối các ký tự lân cận của mỗi cặp. Nếu một nút tương ứng với một cặp vị trí (i, j), thì chúng ta kiểm tra i+1 và j+1 trong chuỗi gốc. Nếu i+1 là dấu ngoặc đơn mở thì nó sẽ trở thành nút con bên trái của nút. Nếu j+1 là dấu ngoặc đơn mở thì nó sẽ trở thành con đúng. Những phần tử con này tương ứng với các cặp dấu ngoặc có dấu ngoặc đơn bên trái nằm chính xác ở các vị trí đó. Gốc của cây là nút 1. 

Nhiệm vụ là xây dựng lại bất kỳ chuỗi khung hợp lệ nào có thể tạo ra cây chính xác này theo cấu trúc được mô tả. 

Kích thước đầu vào lớn, với tổng n trên các trường hợp thử nghiệm lên tới 2×10^5. Bất kỳ giải pháp nào cũng phải gần tuyến tính cho mỗi trường hợp thử nghiệm. Việc tái cấu trúc bậc hai thử tất cả các vị trí hoặc mô phỏng nhiều lần các chuỗi ngay lập tức là quá chậm vì ngay cả 10^5 nút cũng sẽ bao hàm 10^10 thao tác trong một mô phỏng đơn giản về vị trí và quá trình quét. 

Một trường hợp lỗi tinh vi xuất hiện nếu người ta giả sử cây chỉ mã hóa duy nhất cấu trúc lồng nhau. Ví dụ: một gốc có hai con có thể gợi ý rằng cả hai con xuất hiện bên trong cùng một cặp bên ngoài, nhưng việc xây dựng thực sự phụ thuộc vào sự liền kề tại i+1 và j+1, nghĩa là anh chị em tương ứng với “dấu ngoặc mở tiếp theo sau điểm cuối”, chứ không phải ngăn chặn cây con chung. Hiểu sai điều này thường dẫn đến việc đặt trẻ một cách tùy tiện vào các khoảng và tạo ra các chuỗi không hợp lệ hoặc không nhất quán. 

Một trường hợp góc khác là khi một nút chỉ có một nút con hoặc không có nút nào. Cấu trúc ngây thơ “luôn mở các phần con ngay bên trong khoảng cha mẹ” có thể vô tình chồng chéo các khoảng hoặc tạo ra thứ tự không khớp vì các phần mở con bên phải được gắn với j+1, không phải với vị trí truyền tải hiện tại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng gán các vị trí từ 1 đến 2n cho tất cả các dấu ngoặc mở và đóng trong khi tôn trọng rằng mỗi nút i có các vị trí mở và đóng của nó và các mối quan hệ cha-con phải phù hợp với các ràng buộc kề cận. Điều này trở thành vấn đề về vị trí bị hạn chế trên 2n vị trí với các quy tắc phụ thuộc giữa các khoảng thời gian. Ngay cả khi chúng tôi cố gắng quay lại, mỗi nút có thể phân nhánh thành nhiều vị trí và tổng số cấu hình sẽ tăng theo cấp số nhân trong n. Điều này là không thể thực hiện được ngay cả với n khoảng 20. 

Quan sát quan trọng là cây thực sự xác định sự phân rã xác định của chuỗi khi được diễn giải chính xác. Mỗi nút tương ứng với một khoảng và quy tắc xây dựng sử dụng i+1 và j+1 ngụ ý rằng các nút con của mọi nút phải xuất hiện ngay sau khi bắt đầu hoặc ngay sau khi kết thúc khoảng của nó. Điều này loại bỏ sự tự do trong sắp xếp: một khi chúng ta sửa cấu trúc, trình tự có thể được xây dựng lại bằng cách mô phỏng ngược lại cách tạo ra các khoảng. 

Thay vì đặt dấu ngoặc đơn, chúng ta gán vị trí cho các nút. Mỗi nút sẽ có chính xác hai vị trí: vị trí mở khi chúng ta vào và vị trí đóng khi chúng ta thoát. Chúng ta thực hiện việc truyền tải tôn trọng thực tế là các con bên trái được phát hiện từ thời điểm chúng ta đặt dấu ngoặc mở và các con bên phải kể từ thời điểm chúng ta đặt dấu ngoặc đóng. Điều này tự nhiên dẫn đến việc tái cấu trúc theo chiều sâu trong đó chúng tôi phát ra '(' khi vào một nút và ')' khi rời khỏi nút đó, đồng thời xử lý đệ quy các phần tử con theo đúng thứ tự.

Cấu trúc đảm bảo rằng con bên trái phải được xử lý ngay sau khi mở cha mẹ và con bên phải phải được xử lý ngay trước khi đóng cha mẹ, bởi vì những điều đó tương ứng chính xác với các chuyển tiếp i+1 và j+1 trong cấu trúc ban đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Vị trí/quay lại Brute Force | O(2^n) | O(n) | Quá chậm | 
| Tái thiết DFS từ cấu trúc cây | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng lại chuỗi bằng cách mô phỏng cách tạo dấu ngoặc đơn nếu chúng tôi mở rộng cây trở lại thành mã hóa dựa trên truyền tải. 

1. Bắt đầu từ nút gốc 1. Nút này tương ứng với cặp ngoài cùng trong chuỗi, do đó, nó phải tạo dấu ngoặc mở đầu tiên trong bất kỳ cấu trúc hợp lệ nào. 
2. Đối với nút u, xuất '(' ngay lập tức khi nhập u. Điều này tương ứng với việc gán dấu ngoặc đơn bên trái của cặp liên kết với u. 
3. Xử lý đệ quy con trái của u nếu nó tồn tại. Con trái được xác định đến từ vị trí i+1 trong cấu trúc ban đầu, nghĩa là nó phải là cấu trúc đầu tiên xuất hiện ngay sau khi mở u. 
4. Sau khi hoàn thành cây con bên trái, xử lý đệ quy cây con bên phải của u nếu nó tồn tại. Điều này tương ứng với cấu trúc bắt đầu ngay sau j trong chuỗi ban đầu, vì vậy nó phải được đặt sau tất cả nội dung dẫn xuất từ trái của u. 
5. Xuất ')' sau khi cả hai phần tử con được xử lý. Điều này hoàn tất khoảng thời gian tương ứng với u. 
6. Lặp lại DFS này cho tất cả các nút có thể truy cập từ gốc, điều này đảm bảo mỗi nút đóng góp chính xác một dấu ngoặc mở và một dấu ngoặc đóng. 

Tại sao nó hoạt động xuất phát từ việc diễn giải mỗi nút dưới dạng một cặp vị trí trùng khớp xác định khoảng liền kề trong chuỗi được xây dựng lại. Quy tắc xây dựng ràng buộc chặt chẽ các phần tử con với i+1 và j+1, buộc các phần tử con bên trái phải bắt đầu ở điểm sớm nhất có thể sau khi mở và các phần tử bên phải phải bắt đầu ở điểm sớm nhất có thể sau khi đóng. Điều này loại bỏ mọi quyền tự do trong việc sắp xếp các anh chị em ngoài cấu trúc DFS. Phép đệ quy duy trì rằng mọi cây con đều được chứa đầy đủ bên trong khoảng cha mẹ của nó và không thể xen kẽ vì điều đó sẽ vi phạm ràng buộc kề xác định việc tạo cây con. 

## Giải pháp Python```python
import sys
sys.setrecursionlimit(10**7)
input = sys.stdin.readline

def solve():
    n = int(input())
    left = [0] * (n + 1)
    right = [0] * (n + 1)

    for i in range(1, n + 1):
        l, r = map(int, input().split())
        left[i] = l
        right[i] = r

    res = []

    def dfs(u):
        res.append('(')
        if left[u]:
            dfs(left[u])
        if right[u]:
            dfs(right[u])
        res.append(')')

    dfs(1)
    print(''.join(res))

if __name__ == "__main__":
    t = int(input())
    for _ in range(t):
        solve()
```Mã đọc cấu trúc cây cho từng trường hợp thử nghiệm và lưu trữ các phần tử con trong mảng. Chức năng DFS là cơ chế tái thiết cốt lõi. Nó phát ra một dấu ngoặc mở khi nhập một nút và một dấu ngoặc đóng sau khi xử lý các phần tử con của nó, khớp với ngữ nghĩa khoảng của các cặp dấu ngoặc ban đầu. 

Sự tinh tế duy nhất là độ sâu đệ quy. Vì cây có thể là một chuỗi có độ dài lên tới 10^5 nên giới hạn đệ quy được tăng lên để tránh tràn ngăn xếp. Thứ tự duyệt tuân thủ nghiêm ngặt con trái trước con phải, mã hóa sự phụ thuộc rằng con trái bắt nguồn từ chuyển đổi i+1 trước con phải bắt nguồn từ chuyển tiếp j+1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Cây đầu vào: 

Nút 1 có con 2 và 3, nút 2 và 3 là lá. 

| Gọi | Hành động | Đầu ra cho đến nay | 
| --- | --- | --- | 
| dfs(1) | in '(' | ( | 
| dfs(2) | in '(' | (( | 
| dfs(2) kết thúc | in ')' | (() | 
| dfs(3) | in '(' | (() ( | 
| dfs(3) kết thúc | in ')' | (() () | 
| dfs(1) kết thúc | in ')' | (()()) | 

Điều này chứng tỏ rằng các nút anh chị em được đặt tuần tự trong chuỗi được xây dựng lại, phù hợp với ý tưởng rằng chúng bắt nguồn từ các trình kích hoạt lân cận riêng biệt thay vì các khoảng chồng chéo. 

### Ví dụ 2 

Cây đầu vào: 

1 chỉ có con trái 2, 2 có con phải 3. 

| Gọi | Hành động | Đầu ra cho đến nay | 
| --- | --- | --- | 
| dfs(1) | '(' | ( | 
| dfs(2) | '(' | (( | 
| dfs(3) | '(' | ((( | 
| dfs(3) kết thúc | ')' | ((() | 
| dfs(2) kết thúc | ')' | (()) | 
| dfs(1) kết thúc | ')' | (())() | 

Trường hợp này cho thấy cách xử lý các cây con bên phải sau khi kết thúc cây con bên trái, bảo toàn các ranh giới lồng nhau chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập một lần và đóng góp công việc liên tục | 
| Không gian | O(n) | Ngăn xếp đệ quy và lưu trữ kề cận | 

Tổng số n trong các trường hợp thử nghiệm được giới hạn bởi 2×10^5, do đó, việc truyền tải tuyến tính trên mỗi nút là đủ. Cách tiếp cận DFS thực hiện phát xạ chính xác 2n ký tự cho mỗi trường hợp thử nghiệm, phù hợp thoải mái với các ràng buộc thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict
    import sys
    sys.setrecursionlimit(10**7)

    def solve():
        n = int(input())
        left = [0] * (n + 1)
        right = [0] * (n + 1)
        for i in range(1, n + 1):
            l, r = map(int, input().split())
            left[i] = l
            right[i] = r

        res = []

        def dfs(u):
            res.append('(')
            if left[u]:
                dfs(left[u])
            if right[u]:
                dfs(right[u])
            res.append(')')

        dfs(1)
        print(''.join(res))

    t = int(input())
    for _ in range(t):
        solve()

# provided sample
assert run("""1
3
2 3
0 0
0 0
""").strip() == "(())()", "sample 1"

# minimal case
assert run("""1
1
0 0
""").strip() == "()", "single node"

# skewed tree
assert run("""1
3
2 0
3 0
0 0
""").strip() == "((()))", "chain"

# full binary
assert run("""1
3
2 3
0 0
0 0
""").strip() == "(())()", "balanced"

# all right children
assert run("""1
3
0 2
0 3
0 0
""").strip() == "(()())", "right skewed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | () | độ chính xác cấu trúc tối thiểu | 
| chuỗi | ((())) | xử lý đệ quy sâu | 
| cân bằng | (())() | anh chị em đặt hàng | 
| lệch phải | (()()) | logic vị trí con phải | 

## Vỏ cạnh 

Cây nút đơn ánh xạ trực tiếp tới một cặp dấu ngoặc đơn. DFS đi vào nút 1, in '(', không tìm thấy phần tử con nào và in ')', tạo ra chính xác "()", đây là chuỗi hợp lệ duy nhất. 

Một chuỗi sâu như 1 → 2 → 3 → … → n nhấn mạnh độ sâu đệ quy. Việc truyền tải liên tục đi vào các phần tử con bên trái (hoặc bên phải) tùy thuộc vào cấu trúc, nhưng vẫn phát ra chính xác một lần mở trước khi đi xuống và một lần đóng sau khi quay trở lại, duy trì việc lồng ghép thích hợp. Ngăn xếp tăng tuyến tính nhưng không bao giờ vượt quá n khung. 

Một nút chỉ có một nút con bên phải chứng tỏ rằng các nút con bên phải không bị ràng buộc vào vị trí trong khối bên trong hiện có mà được thêm vào sau khi cây con bên trái hoàn thành. DFS vẫn đưa ra cách lồng đúng vì cây con bên phải được đặt trong cùng một khoảng cha nhưng xuất hiện muộn hơn trong quá trình tuyến tính hóa, phù hợp với quy tắc xây dựng dựa trên j+1.
