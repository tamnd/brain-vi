---
title: "CF 103081A - Lòng biết ơn"
description: "Chúng tôi được cung cấp một bản ghi theo trình tự thời gian về những ghi chú biết ơn hàng ngày của Ben. Mỗi ngày đóng góp chính xác ba mục văn bản độc lập và do đó trong tất cả các ngày, chúng tôi có tổng cộng một chuỗi gồm 3N chuỗi. Mỗi chuỗi đại diện cho một “điều” Ben đã viết ra."
date: "2026-07-03T23:16:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103081
codeforces_index: "A"
codeforces_contest_name: "2020-2021 ICPC Southwestern European Regional Contest (SWERC 2020)"
rating: 0
weight: 103081
solve_time_s: 49
verified: true
draft: false
---

[CF 103081A - Lòng biết ơn](https://codeforces.com/problemset/problem/103081/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bản ghi theo trình tự thời gian về những ghi chú biết ơn hàng ngày của Ben. Mỗi ngày đóng góp chính xác ba mục văn bản độc lập và do đó trong tất cả các ngày, chúng tôi có tổng cộng một chuỗi gồm 3N chuỗi. Mỗi chuỗi đại diện cho một “điều” Ben đã viết ra. 

Nhiệm vụ là tóm tắt bộ sưu tập này bằng cách xác định K chuỗi xuất hiện thường xuyên nhất. Chỉ tần suất thôi thì không đủ để xác định đầy đủ thứ tự. Khi hai chuỗi khác nhau xuất hiện với số lần giống nhau, chuỗi có lần xuất hiện cuối cùng xảy ra sau trong đầu vào phải xuất hiện trước. 

Đầu ra là một danh sách được xếp hạng gồm các chuỗi riêng biệt, được sắp xếp chủ yếu theo tần suất giảm dần và thứ hai là bằng cách giảm chỉ số xuất hiện cuối cùng và chúng tôi chỉ in K trên cùng của chúng. 

Kích thước đầu vào lên tới 100.000 dòng văn bản. Bất kỳ giải pháp nào liên tục sắp xếp hoặc quét toàn bộ tập dữ liệu trên mỗi chuỗi sẽ quá chậm. Ràng buộc tự nhiên gợi ý rằng chúng ta nên nhắm tới thứ gì đó gần với O(N log N) hoặc O(N), vì O(N^2) trên chuỗi là không thể. 

Một vấn đề tế nhị là thứ tự không chỉ phụ thuộc vào số lượng mà còn phụ thuộc vào vị trí xuất hiện cuối cùng. Điều này có nghĩa là chúng ta phải theo dõi cả hai tổng hợp trong một lần chuyển. Một bản đồ tần số đơn giản là không đủ trừ khi nó cũng lưu trữ thông tin vị trí. 

Một số trường hợp đặc biệt quan trọng: 

Nếu tất cả các chuỗi là duy nhất thì mỗi tần số là 1, do đó thứ tự phụ thuộc hoàn toàn vào lần xuất hiện cuối cùng, điều này thực sự trở thành thứ tự đầu vào đảo ngược của các phần tử duy nhất. 

Nếu tất cả các chuỗi giống hệt nhau thì chỉ có một dòng đầu ra bất kể K. 

Nếu K vượt quá số lượng chuỗi riêng biệt, chúng tôi sẽ xuất ra tất cả các chuỗi riêng biệt. 

Một sai lầm ngây thơ là chỉ sắp xếp theo tần suất và bỏ qua quy tắc ràng buộc. Ví dụ: nếu cả "A" và "B" đều xuất hiện hai lần, nhưng "A" xuất hiện lần cuối trước "B" thì "B" phải xuất hiện trước ngay cả khi số lượng của chúng bằng nhau. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: trước tiên hãy thu thập tất cả các chuỗi 3N, sau đó đối với mỗi chuỗi riêng biệt, hãy quét lại toàn bộ danh sách để đếm số lần xuất hiện và theo dõi vị trí cuối cùng của nó. Sau đó, sắp xếp tất cả các chuỗi riêng biệt bằng cách sử dụng các giá trị được tính toán này. 

Điều này đúng, nhưng cực kỳ tốn kém. Nếu có U chuỗi riêng biệt, mỗi chuỗi yêu cầu quét O(N), chỉ riêng việc xử lý trước đã tốn O(UN). Trong trường hợp xấu nhất U là Θ(N), cho ra O(N2), điều này vượt xa khả năng thực hiện đối với N lên tới 100.000. 

Quan sát quan trọng là cả số liệu thống kê cần thiết, tần suất và chỉ số xuất hiện cuối cùng đều có thể được tính toán trong một lần quét tuyến tính. Thay vì tính toán lại số lượng trên mỗi chuỗi, chúng tôi duy trì bản đồ băm từ chuỗi đến một cặp: số lượng của nó và chỉ mục cuối cùng nơi nó xuất hiện. Mỗi bản cập nhật có giá trị trung bình là O(1), do đó quá trình tiền xử lý đầy đủ là O(N). 

Khi chúng tôi có bản đồ này, chúng tôi chuyển đổi nó thành một danh sách và sắp xếp nó một lần bằng cách sử dụng khóa thứ tự bắt buộc: tần số cao hơn trước, sau đó là chỉ số cuối cùng cao hơn trước. Điều này làm giảm vấn đề thành một nhiệm vụ sắp xếp tiêu chuẩn trên các mục U. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(N) | Quá chậm | 
| Tối ưu | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Các bước

1. Đọc tất cả các chuỗi 3N theo thứ tự, giữ chỉ số i đang chạy từ 0 đến 3N−1. Chỉ số này rất cần thiết vì sự ràng buộc phụ thuộc vào vị trí xuất hiện cuối cùng. 
2. Duy trì một từ điển được khóa bằng chuỗi. Đối với mỗi chuỗi s ở vị trí i, hãy cập nhật mục nhập của nó bằng cách tăng tần số của nó và đặt vị trí cuối cùng của nó thành i. Ghi đè vị trí cuối cùng là chính xác vì chúng tôi luôn muốn lần xuất hiện gần đây nhất. 
3. Sau khi xử lý tất cả các dòng, hãy chuyển đổi từ điển thành danh sách các bộ dữ liệu có dạng (tần số, vị trí cuối cùng, chuỗi). Định dạng này giúp việc sắp xếp trở nên đơn giản. 
4. Sắp xếp danh sách này theo thứ tự tùy chỉnh: tần số cao hơn trước và đối với tần số bằng nhau, vị trí cuối cùng cao hơn trước. Bản thân chuỗi chỉ được mang cho đầu ra. 
5. Xuất K chuỗi đầu tiên từ danh sách đã sắp xếp. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến rằng sau khi xử lý dòng thứ i, mọi chuỗi trong từ điển sẽ lưu trữ chính xác tổng tần số của nó trong tiền tố [0, i] và chỉ số xuất hiện gần đây nhất của nó trong tiền tố đó. Vì mọi cập nhật đều mang tính cục bộ và chỉ ghi đè vị trí cuối cùng nên không có thông tin lịch sử nào bị mất có thể ảnh hưởng đến tính chính xác. Sau lần lặp cuối cùng, các giá trị này chính xác cho toàn bộ chuỗi, do đó, việc sắp xếp chúng trên toàn cầu sẽ tạo ra thứ hạng được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    
    freq = {}
    last = {}
    
    for i in range(3 * n):
        s = input().rstrip('\n')
        if s in freq:
            freq[s] += 1
        else:
            freq[s] = 0
            freq[s] = 1
        last[s] = i
    
    items = []
    for s in freq:
        items.append((freq[s], last[s], s))
    
    items.sort(key=lambda x: (-x[0], -x[1]))
    
    out = []
    for i in range(min(k, len(items))):
        out.append(items[i][2])
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng hai từ điển, một cho tần suất và một cho chỉ số lần xuất hiện cuối cùng. Chúng có thể được hợp nhất thành một cấu trúc duy nhất, nhưng việc giữ chúng tách biệt sẽ làm cho logic trở nên rõ ràng: một theo dõi số lượng, một theo dõi lần truy cập gần đây. 

Phím sắp xếp trực tiếp mã hóa quy tắc sắp xếp của bài toán. Việc phủ định cả tần số và chỉ mục cuối cùng sẽ biến sắp xếp tăng dần mặc định thành thứ tự giảm dần bắt buộc. 

Một điểm tinh tế là chúng tôi lưu trữ chỉ mục cuối cùng dưới dạng bộ đếm vòng lặp i trên các dòng đầu vào thô. Điều này đảm bảo so sánh chính xác về mặt thời gian ngay cả khi các ngày được nhóm thành ba. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
A
B
C
D
C
E
```Chúng tôi xử lý 6 mục. 

| tôi | chuỗi | trạng thái tần số | trạng thái cuối cùng | 
| --- | --- | --- | --- | 
| 0 | A | Đ:1 | Đáp: 0 | 
| 1 | B | B:1 | B:1 | 
| 2 | C | C:1 | C:2 | 
| 3 | D | D:1 | Đ:3 | 
| 4 | C | C:2 | C:4 | 
| 5 | E | E:1 | E:5 | 

Mục cuối cùng trước khi sắp xếp: 

A(1,0), B(1,1), C(2,4), D(1,3), E(1,5) 

Đã sắp xếp: 

C(2,4), E(1,5), D(1,3), B(1,1), A(1,0) 

Lấy K=2 ta có: 

C 

E 

Điều này cho thấy tần suất chiếm ưu thế, trong khi các mối quan hệ được giải quyết bằng chỉ số xuất hiện cuối cùng. 

### Ví dụ 2 

đầu vào:```
1 5
X
Y
Z
```Tất cả tần số là 1, vị trí cuối cùng là X:0, Y:1, Z:2. 

Thứ tự sắp xếp trở thành Z, Y, X. 

Vì K=5 nhưng chỉ tồn tại 3 mục riêng biệt nên chúng tôi xuất tất cả chúng. 

Điều này xác nhận rằng K hoạt động như một giới hạn trên thay vì buộc phải đệm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Một lần xây dựng bản đồ băm trong O(N), sắp xếp U 3N mục có giá O(N log N) | 
| Không gian | O(N) | Lưu trữ tần số và vị trí cuối cùng cho mỗi chuỗi riêng biệt | 

Giới hạn đầu vào là 100.000 dòng khiến phương pháp này trở nên an toàn một cách thoải mái. Bước băm là tuyến tính và cách sắp xếp chiếm ưu thế nhưng vẫn nằm trong các ràng buộc điển hình đối với Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys
    from io import StringIO
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = StringIO(inp)
    sys.stdout = StringIO()
    
    def solve():
        n, k = map(int, input().split())
        freq = {}
        last = {}
        for i in range(3 * n):
            s = input().rstrip('\n')
            if s in freq:
                freq[s] += 1
            else:
                freq[s] = 1
            last[s] = i
        
        items = [(freq[s], last[s], s) for s in freq]
        items.sort(key=lambda x: (-x[0], -x[1]))
        k2 = min(k, len(items))
        print("\n".join(items[i][2] for i in range(k2)))
    
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return out.strip()

# provided samples
assert solve_capture("""2 2
A
B
C
D
C
E
""") == "C\nE"

# all equal
assert solve_capture("""3 2
A
A
A
A
A
A
A
A
A
""") == "A"

# all distinct
assert solve_capture("""1 3
A
B
C
""") == "C\nB\nA"

# tie by last occurrence
assert solve_capture("""1 3
A
B
A
""") == "A\nB"

# K exceeds distinct
assert solve_capture("""1 10
X
Y
Z
""") == "Z\nY\nX"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các chuỗi bằng nhau | dòng đơn | sự sụp đổ của các bản sao | 
| tất cả đều khác biệt | thứ tự ngược lại | hòa theo thời gian gần đây | 
| lặp lại với cà vạt | A trước B | quy tắc xuất hiện lần cuối | 
| K > khác biệt | danh sách đầy đủ | hành vi cắt ngắn | 

## Vỏ cạnh 

Một trường hợp thất bại phổ biến là bỏ qua quy tắc xuất hiện cuối cùng. Coi như:```
1 2
A
B
A
```Ở đây A và B đều xuất hiện một lần, nhưng A phải đứng trước B vì lần xuất hiện cuối cùng của nó là ở chỉ số 2, trong khi B ở chỉ mục 1. Thuật toán cập nhật chính xác các vị trí cuối cùng trong quá trình quét nên A được ưu tiên cao hơn. 

Một trường hợp cạnh khác là khi K vượt quá số lượng chuỗi duy nhất. Bước sắp xếp chỉ tạo ra các mục riêng biệt, do đó, việc cắt bằng min(k, len(items)) sẽ ngăn kết quả đầu ra nằm ngoài phạm vi.
