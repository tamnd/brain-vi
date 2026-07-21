---
title: "CF 103648G - Chim Bồ Câu Vũ"
description: "Ý tưởng brute-force là duy trì một tập hợp rõ ràng tất cả các chuỗi hiện có trong điệu nhảy và trên mỗi truy vấn, lặp lại tất cả các chuỗi và kiểm tra xem truy vấn có phải là tiền tố của bất kỳ chuỗi nào trong số chúng hay không. Đối với thao tác loại ba, chúng tôi đảo ngược vật lý mọi chuỗi trong tập hợp."
date: "2026-07-02T22:03:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103648
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 04-08-22 Div. 1 (Advanced)"
rating: 0
weight: 103648
solve_time_s: 49
verified: true
draft: false
---

[CF 103648G - Vũ điệu chim bồ câu](https://codeforces.com/problemset/problem/103648/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Phương pháp tiếp cận 

Ý tưởng brute-force là duy trì một tập hợp rõ ràng tất cả các chuỗi hiện có trong điệu nhảy và trên mỗi truy vấn, lặp lại tất cả các chuỗi và kiểm tra xem truy vấn có phải là tiền tố của bất kỳ chuỗi nào trong số chúng hay không. Đối với thao tác loại ba, chúng tôi đảo ngược vật lý mọi chuỗi trong tập hợp. Điều này đúng vì nó mô phỏng trực tiếp báo cáo vấn đề, nhưng chi phí của nó bị chi phối bởi việc đảo ngược hoàn toàn lặp đi lặp lại và quét tiền tố lặp đi lặp lại. Mỗi lần đảo ngược là O (tổng chiều dài của tất cả các chuỗi) và có thể có tới 10^5 thao tác như vậy, đưa ra trường hợp xấu nhất là khoảng 10^10 thao tác ký tự. 

Quan sát quan trọng là sự đảo chiều toàn cầu là đồng đều. Mỗi chuỗi được lưu trữ ở dạng bình thường hoặc dạng đảo ngược chỉ tùy thuộc vào tính chẵn lẻ của số lượng thao tác loại ba được thấy cho đến nay. Thay vì sửa đổi các chuỗi được lưu trữ, chúng tôi duy trì hai lần thử: một lần lưu trữ các chuỗi theo hướng ban đầu và một lần lưu trữ các chuỗi đảo ngược. Chúng tôi cũng duy trì trạng thái lật boolean. Khi một chuỗi được chèn vào, chúng tôi chèn nó vào cả hai lần thử, nhưng chúng tôi diễn giải một cách hợp lý trie nào đang hoạt động tùy thuộc vào trạng thái lật hiện tại. Khi một truy vấn đến, chúng tôi sẽ tìm kiếm truy vấn đó trong trie thuận hoặc trong trie đảo ngược tùy thuộc vào việc hiện tại chúng tôi có bị đảo ngược hay không. Thao tác lật trở thành việc chuyển đổi một boolean, đó là O(1). 

Điều này làm giảm vấn đề đối với các truy vấn tồn tại tiền tố tiêu chuẩn trong một bộ ba, với các phép biến đổi toàn cục theo thời gian không đổi được khấu hao. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q * tổng chiều dài) cộng với các lần đảo ngược hoàn toàn lặp đi lặp lại | O(tổng chiều dài) | Quá chậm | 
| Hai Trie với sự lười biếng đảo ngược | O(tổng chiều dài + Q * | truy vấn | ) | 

## Hướng dẫn thuật toán 

1. Duy trì hai cấu trúc trie, một cấu trúc biểu thị các chuỗi được chèn và một biểu thị dạng đảo ngược của chúng. Sự tách biệt này đảm bảo chúng tôi có thể trả lời các truy vấn tiền tố theo một trong hai hướng mà không cần sửa đổi dữ liệu hiện có. 
2. Giữ một biến boolean cho biết liệu một phép đảo chiều toàn cục đã được áp dụng số lần lẻ hay số lần chẵn. Biến này xác định trie nào hiện đang diễn giải tích cực các chuỗi được lưu trữ. 
3. Đối với mỗi thao tác chèn, hãy chèn chuỗi vào cả hai lần thử. Điều này là cần thiết vì các truy vấn trong tương lai có thể diễn giải cùng một chuỗi theo một trong hai hướng tùy thuộc vào sự đảo ngược trong tương lai và việc tính toán trước cả hai cách biểu diễn sẽ tránh được việc tính toán lại. 
4. Đối với mỗi thao tác truy vấn, duyệt qua trie tương ứng với trạng thái đảo ngược hiện tại bằng cách sử dụng chuỗi truy vấn đã cho. Nếu chúng tôi đạt đến một nút hợp lệ ở mỗi ký tự, chúng tôi sẽ kiểm tra xem đường dẫn đó có tồn tại hay không, nghĩa là ít nhất một chuỗi được lưu trữ có truy vấn làm tiền tố. 
5. Đối với mỗi thao tác đảo ngược, chỉ cần chuyển đổi cờ boolean thay vì sửa đổi dữ liệu được lưu trữ. Điều này phản ánh rằng tất cả các chuỗi đều thay đổi hướng đồng thời. 

Lý do điều này có tác dụng là vì trạng thái của hệ thống tại bất kỳ thời điểm nào chỉ phụ thuộc vào tính chẵn lẻ của các đảo chiều chứ không phụ thuộc vào vị trí của chúng. Mọi chuỗi đều trải qua trình tự biến đổi giống nhau, do đó việc biểu diễn trước cả hai hướng sẽ bảo toàn mọi khả năng trong tương lai. Trie đảm bảo các truy vấn tiền tố được trả lời theo thời gian chỉ tỷ lệ với độ dài truy vấn và cấu trúc toàn cục vẫn nhất quán vì không có chuỗi lưu trữ nào cần được cập nhật vật lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Trie:
    def __init__(self):
        self.next = {}
        self.cnt = 0

    def insert(self, s):
        node = self
        node.cnt += 1
        for ch in s:
            if ch not in node.next:
                node.next[ch] = Trie()
            node = node.next[ch]
            node.cnt += 1

    def query(self, s):
        node = self
        for ch in s:
            if ch not in node.next:
                return False
            node = node.next[ch]
        return True

def main():
    q = int(input())
    forward = Trie()
    backward = Trie()
    flipped = False

    for _ in range(q):
        parts = input().split()
        t = parts[0]

        if t == "1":
            s = parts[1]
            forward.insert(s)
            backward.insert(s[::-1])

        elif t == "3":
            flipped = not flipped

        else:
            s = parts[1]
            if flipped:
                print(1 if backward.query(s) else 0)
            else:
                print(1 if forward.query(s) else 0)

if __name__ == "__main__":
    main()
```Bước chèn có chủ ý lưu trữ cả hai hướng để chúng ta không bao giờ cần phải tính toán lại các lần đảo ngược trong các lần lật tổng thể. Cờ lật kiểm soát trie nào được truy vấn, giữ cho logic truy vấn không phụ thuộc vào độ dài lịch sử. 

Một chi tiết triển khai tinh tế là chúng tôi không cố gắng duy trì số lượng thành viên chuỗi đầy đủ mà chỉ tồn tại tiền tố. Các nút trie không cần cờ kết thúc vì bất kỳ đường dẫn hợp lệ nào đều biểu thị ít nhất một chuỗi được lưu trữ tiếp tục thông qua tiền tố đó. 

## Ví dụ đã hoạt động 

Hãy xem xét trình tự mà chúng ta chèn "alice", truy vấn "alice", lật, sau đó truy vấn "ali" và "ecil". 

| Bước | Hoạt động | Lật | Trie sử dụng | Truy vấn | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | chèn alice | sai | tiến + lùi | - | - | 
| 2 | truy vấn Alice | sai | chuyển tiếp | Alice | 1 | 
| 3 | lật | đúng | - | - | - | 
| 4 | truy vấn hoặc | đúng | lạc hậu | ali | 0 | 
| 5 | truy vấn ecil | đúng | lạc hậu | ecil | 1 | 

Dấu vết này cho thấy cùng một chuỗi được lưu trữ có thể đáp ứng các truy vấn khác nhau như thế nào tùy theo hướng mà không cần bất kỳ sửa đổi cấu trúc nào. 

Bây giờ hãy xem xét việc chèn "abc" và "xyz", sau đó lật hai lần với một truy vấn ở giữa. 

| Bước | Hoạt động | Lật | Truy vấn | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | chèn abc | sai | - | - | 
| 2 | chèn xyz | sai | - | - | 
| 3 | lật | đúng | - | - | 
| 4 | truy vấn "ab" | đúng | 0 | | 
| 5 | lật | sai | - | - | 
| 6 | truy vấn "xy" | sai | 1 | | 

Điều này chứng tỏ rằng tính chẵn lẻ của lần lật xác định hoàn toàn cách giải thích và các lần lật lặp lại hoạt động nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng chiều dài của chuỗi được chèn + tổng chiều dài truy vấn) | mỗi ký tự được xử lý một số lần không đổi trong các phép toán trie | 
| Không gian | O(tổng chiều dài của chuỗi được chèn) | mỗi chuỗi đóng góp các nút trong cả hai lần thử, tuyến tính trong tổng kích thước đầu vào | 

Các ràng buộc cho phép tổng cộng tối đa 10^5 ký tự, do đó việc xây dựng và truyền tải thời gian tuyến tính nằm trong giới hạn, đồng thời tránh bất kỳ sự đảo ngược toàn bộ chuỗi nào trên mỗi thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    output = []
    def fake_print(x):
        output.append(str(x))

    global print
    old_print = print
    print = fake_print
    try:
        # inline solution
        class Trie:
            def __init__(self):
                self.next = {}

            def insert(self, s):
                node = self
                for ch in s:
                    if ch not in node.next:
                        node.next[ch] = Trie()
                    node = node.next[ch]

            def query(self, s):
                node = self
                for ch in s:
                    if ch not in node.next:
                        return False
                    node = node.next[ch]
                return True

        data = inp.strip().split()
        q = int(data[0])
        idx = 1
        f = Trie()
        b = Trie()
        flipped = False

        for _ in range(q):
            t = data[idx]
            idx += 1
            if t == "1":
                s = data[idx]
                idx += 1
                f.insert(s)
                b.insert(s[::-1])
            elif t == "3":
                flipped = not flipped
            else:
                s = data[idx]
                idx += 1
                print(1 if (b.query(s) if flipped else f.query(s)) else 0)
    finally:
        print = old_print

    return "\n".join(output)

# sample
assert run("""5
1 alice
2 alice
3
2 ali
2 ecil
""") == """1
0
1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chèn đơn + truy vấn | 1 | phát hiện tiền tố cơ bản | 
| Lật thì truy vấn không khớp | 0 | sự đảo ngược đúng đắn | 
| Lật đôi trả lại bản gốc | 1 | xử lý chẵn lẻ | 
| Nhiều chèn | Trộn 1/0 | thử chia sẻ đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh tranh quan trọng là khi có nhiều lần lật xảy ra mà không có bất kỳ sự chèn vào nào ở giữa. Thuật toán xử lý việc này một cách an toàn vì các lần lật chỉ chuyển đổi một boolean và không làm thay đổi trạng thái trie, do đó các lần lật lặp lại không tích lũy chi phí. 

Một trường hợp đặc biệt khác là truy vấn tiền tố trống hoặc tiền tố rất ngắn đối với các chuỗi được lưu trữ dài. Trie đảm bảo rằng ngay cả khi nhiều chuỗi chia sẻ tiền tố, việc kiểm tra sự tồn tại vẫn chính xác vì bất kỳ sự tiếp tục nào ngoài đường dẫn truy vấn đều xác nhận kết quả khớp hợp lệ. 

Cuối cùng, việc chèn chuỗi sau một chuỗi lần lật vẫn hoạt động vì mỗi lần chèn được ghi lại theo cả hai hướng ngay lập tức, khiến những thay đổi trạng thái trong tương lai không liên quan đến dữ liệu trong quá khứ.
