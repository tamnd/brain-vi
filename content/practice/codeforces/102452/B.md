---
title: "CF 102452B - Cây nhị phân"
description: "Chúng ta có một cây nhị phân có gốc với nút 1 là gốc của nó. Alice và Bob lần lượt loại bỏ một cây con. Việc di chuyển chỉ hợp pháp khi cây con bị loại bỏ là cây nhị phân đầy đủ hoàn hảo, nghĩa là mỗi nút bên trong có chính xác hai nút con và tất cả các lá đều có cùng độ sâu."
date: "2026-08-14T15:57:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102452
codeforces_index: "B"
codeforces_contest_name: "2019-2020 ICPC Asia Hong Kong Regional Contest"
rating: 0
weight: 102452
solve_time_s: 56
verified: true
draft: false
---

[CF 102452B - Cây nhị phân](https://codeforces.com/problemset/problem/102452/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây nhị phân có gốc với nút 1 là gốc của nó. Alice và Bob lần lượt loại bỏ một cây con. Việc di chuyển chỉ hợp pháp khi cây con bị loại bỏ là cây nhị phân đầy đủ hoàn hảo, nghĩa là mỗi nút bên trong có chính xác hai nút con và tất cả các lá đều có cùng độ sâu. Cây con bị loại bỏ có thể là toàn bộ cây nên cây có thể trở nên trống. 

Người chơi không có động thái hợp pháp sẽ thua. Vì mọi cây nhị phân khác trống đều có ít nhất một lá và một lá riêng lẻ là cây nhị phân đầy đủ hoàn hảo nên mọi vị trí khác trống luôn có ít nhất một nước đi hợp lệ. Do đó, trò chơi kết thúc chính xác khi mọi nút đã bị loại bỏ. 

Đầu vào chứa một số trường hợp thử nghiệm độc lập. Đối với mỗi trường hợp, các cạnh mô tả cây nhị phân có gốc, với nút 1 là gốc. Nhiệm vụ chỉ đơn giản là xác định xem người chơi thứ nhất, Alice hay người chơi thứ hai, Bob, sẽ thắng trong lối chơi tối ưu. 

Số nút trong một trường hợp thử nghiệm nhiều nhất là 5000 và tổng số nút trên tất cả các trường hợp thử nghiệm nhiều nhất là 50000. Giới hạn này là quá đủ cho một giải pháp thời gian tuyến tính, nhưng nó loại trừ các phương pháp liệt kê một tập hợp lớn các trạng thái trò chơi có thể có. Trên thực tế, bản thân cấu trúc cây hóa ra không quan trọng chút nào, vì vậy chúng ta có thể tránh việc đi ngang qua các cạnh. 

Có hai trường hợp cạnh rất dễ xử lý sai. Đầu tiên, một cây chỉ chứa gốc của nó có một nút và gốc đó là cây nhị phân đầy đủ hoàn hảo. Đối với đầu vào```
11
```Alice loại bỏ gốc ngay lập tức, vì vậy câu trả lời là`Alice`. Việc triển khai giả định rằng mỗi lần di chuyển sẽ loại bỏ ít nhất hai nút sẽ mắc lỗi này. 

Trường hợp cạnh thứ hai là một cây có kích thước chẵn trong đó người chơi có thể loại bỏ một cây con hoàn hảo lớn. Ví dụ,```
141 21 33 4
```Câu trả lời là`Bob`. Alice không thể buộc chiến thắng chỉ bằng cách chọn một cây con cụ thể. Mỗi lần loại bỏ hợp pháp đều chứa một số nút lẻ, vì vậy sau mỗi lần di chuyển, tính chẵn lẻ của số nút còn lại sẽ bị lật. Vì toàn bộ cây có bốn nút, nên phải thực hiện chính xác một số lần di chuyển chẵn trước khi cây trở nên trống, bất kể cây con hợp lệ nào được chọn. 

## Phương pháp tiếp cận 

Một giải pháp lý thuyết trò chơi trực tiếp sẽ kiểm tra đệ quy mọi nước đi hợp pháp. Đối với mỗi cây kết quả, chúng tôi sẽ xác định xem người chơi tiếp theo có chiến thắng hay không bằng cách kiểm tra đệ quy các nước đi hợp pháp của người đó. Điều này đúng vì một thế cờ đang thắng chính xác khi nó có ít nhất một nước đi dẫn đến thế thua. 

Vấn đề là số lượng các vị trí khác nhau. Một thao tác sẽ xóa toàn bộ cây con có gốc và các trình tự xóa khác nhau có thể tạo ra nhiều cây còn lại khác nhau. Một tìm kiếm brute-force có thể có nhiều trạng thái theo cấp số nhân. Có thể có tới 2 n tập hợp con khác nhau của các nút trong không gian trạng thái rộng nhất có thể được giới hạn và việc kiểm tra từng trạng thái có thể yêu cầu quét cây. Do đó, việc triển khai đơn giản có thể đạt được công việc O(n2 n ), điều này hoàn toàn không thực tế ngay cả với n=5000. 

Lực lượng vũ phu hoạt động vì nó mô hình hóa rõ ràng mọi lựa chọn có thể, nhưng trò chơi có tính bất biến mạnh hơn nhiều. 

Mỗi bước di chuyển hợp pháp sẽ loại bỏ một cây nhị phân đầy đủ hoàn hảo. Nếu cây như vậy có chiều cao h thì số nút của nó là 

1+2+4+⋯+2 h =2 h+1 −1. 

Số này luôn là số lẻ. Do đó, mỗi lần di chuyển sẽ loại bỏ một số nút lẻ. 

Có một quan sát quan trọng hơn. Cây nhị phân khác rỗng luôn chứa một lá và lá là cây nhị phân đầy đủ hoàn hảo có chiều cao bằng 0. Vì vậy, luôn có ít nhất một nước đi hợp lệ trong khi cây không rỗng. Trò chơi chỉ có thể dừng khi cây không còn nút nào. 

Giả sử trò chơi kéo dài k nước đi. Mỗi lần di chuyển sẽ loại bỏ một số nút lẻ, do đó tổng số nút bị loại bỏ có cùng tính chẵn lẻ với k. Vì cuối cùng tất cả n nút đều bị loại bỏ, 

k≡n(mod2). 

Alice thắng chính xác khi số nước đi là số lẻ, vì Alice đi trước. Do đó Alice thắng chính xác khi n lẻ. 

Hình dạng thực tế của cây, các cây con hoàn hảo có sẵn và lựa chọn của người chơi không liên quan đến người chiến thắng. Chúng tôi chỉ cần tính chẵn lẻ của số lượng nút. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2 n ) trong tìm kiếm trạng thái đơn giản | O(n2 n ) | Quá chậm | 
| Tối ưu | Xử lý đầu vào O(n), logic trò chơi O(1) | O(1) ngoài bộ nhớ đầu vào | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số n nút trong cây hiện tại. Các cạnh không ảnh hưởng đến câu trả lời nhưng chúng vẫn phải được sử dụng từ đầu vào. 
2. Nếu n lẻ, in`Alice`. Tổng số nước đi phải là số lẻ nên người chơi đầu tiên sẽ là người thực hiện nước đi cuối cùng. 
3. Nếu n chẵn thì in`Bob`. Tổng số nước đi phải là số chẵn nên người chơi thứ hai sẽ là người thực hiện nước đi cuối cùng. 
4. Lặp lại quy trình tương tự cho mọi trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Mỗi nước đi hợp lệ sẽ loại bỏ một cây nhị phân đầy đủ hoàn hảo, có số nút là 2 h+1 −1, luôn là số lẻ. Cây nhị phân khác rỗng luôn chứa một lá, do đó, một nước đi hợp lệ luôn tồn tại cho đến khi cây trở nên trống. Do đó, nếu trò chơi kéo dài k nước đi, tổng của k số lẻ bằng n ban đầu, cho ra k≡n(mod2). Alice thực hiện các nước đi thứ nhất, thứ ba, thứ năm và các nước lẻ khác, vì vậy cô ấy thắng chính xác khi k lẻ, tức là khi n lẻ. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def solve():    t = int(input())    ans = []
    for _ in range(t):        n = int(input())
        # The edges do not affect the answer, but must be consumed.        for _ in range(n - 1):            input()
        if n & 1:            ans.append("Alice")        else:            ans.append("Bob")
    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":    solve()
```Dòng đầu tiên ghi số lượng test. Đối với mỗi trường hợp, chúng ta đọc n, sau đó sử dụng chính xác n−1 dòng cạnh vì đầu vào mô tả một cây. 

Kiểm tra tính chẵn lẻ`n & 1`kiểm tra xem số lượng nút có phải là số lẻ hay không. Không cần thiết phải xây dựng danh sách kề, root cây, tính toán kích thước cây con hoặc xác định cây con hoàn hảo. Tất cả những hoạt động đó sẽ giải quyết thông tin mà bất biến chẵn lẻ làm cho không liên quan. 

Trường hợp một nút được xử lý một cách tự nhiên. Với n=1,`n & 1`là đúng, vì vậy chương trình sẽ in`Alice`. 

Cũng không có vấn đề tràn số nguyên. Các số nguyên Python có thể biểu thị trực tiếp đầu vào và thuật toán chỉ thực hiện kiểm tra tính chẵn lẻ. Vòng lặp đọc cạnh là cần thiết ngay cả khi các cạnh không được sử dụng, vì việc để chúng không được đọc sẽ làm hỏng vị trí đầu vào cho trường hợp kiểm thử tiếp theo. 

## Ví dụ đã hoạt động 

Chỉ có một mẫu được cung cấp trong câu lệnh, vì vậy dấu vết thứ hai sử dụng một cây tùy chỉnh nhỏ. 

### Mẫu 1 

Cây mẫu có năm nút.```
151 21 33 43 5
```Trạng thái liên quan chỉ là số lượng nút. 

| Bước | Các nút còn lại | Chẵn lẻ | Kết quả | 
| --- | --- | --- | --- | 
| Ban đầu | 5 | lẻ | Alice thắng | 

Cây chứa một cây con hoàn hảo có gốc tại nút 3 với ba nút. Alice có thể loại bỏ nó, để lại nút 1 và 2. Bob có thể loại bỏ nút 2, để lại nút 1. Alice sau đó loại bỏ nút 1. Trò chơi thực hiện ba bước, khớp với số chẵn lẻ của năm nút ban đầu. 

### Ví dụ tùy chỉnh 2 

Hãy xem xét một cây bốn nút.```
141 21 33 4
```Dấu vết là: 

| Bước | Các nút còn lại | Chẵn lẻ | Người chơi di chuyển | 
| --- | --- | --- | --- | 
| 0 | 4 | Thậm chí | Alice | 
| 1 | 3 | lẻ | Bob | 
| 2 | 1 | lẻ | Alice | 
| 3 | 0 | Thậm chí | Bob | 

Mỗi lần di chuyển sẽ loại bỏ một số nút lẻ. Trong trình tự cụ thể này, người chơi loại bỏ từng nút một, thực hiện bốn nước đi. Bob thực hiện nước thứ tư và thắng trò chơi. Các lựa chọn pháp lý khác có thể thay đổi quy mô của từng lần xóa nhưng không thể thay đổi tính chẵn lẻ của tổng số lần di chuyển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng ta chỉ đọc n-1 cạnh và thực hiện một lần kiểm tra tính chẵn lẻ. | 
| Không gian | O(1) không gian phụ trợ | Cây không bao giờ được lưu trữ. | 

Trên tất cả các trường hợp thử nghiệm, tổng số nút nhiều nhất là 50000, do đó tổng thời gian chạy là O(50000) ngoài việc xử lý đầu vào. Điều này nằm trong giới hạn sẵn có và thuật toán sử dụng bộ nhớ phụ không đổi. 

## Trường hợp thử nghiệm```python
Pythonimport sysimport io

def solve():    input = sys.stdin.readline    t = int(input())    ans = []
    for _ in range(t):        n = int(input())        for _ in range(n - 1):            input()
        ans.append("Alice" if n & 1 else "Bob")
    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:    old_stdin = sys.stdin    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)    sys.stdout = io.StringIO()
    try:        solve()        return sys.stdout.getvalue()    finally:        sys.stdin = old_stdin        sys.stdout = old_stdout

# Provided sampleassert run(    """151 21 33 43 5""") == "Alice\n", "sample 1"
# Minimum-size treeassert run(    """11""") == "Alice\n", "single root is a legal perfect subtree"
# Smallest even treeassert run(    """121 2""") == "Bob\n", "one move removes one node, leaving another"
# Perfect tree with 7 nodesassert run(    """171 21 32 42 53 63 7""") == "Alice\n", "perfect tree with odd size"
# Maximum-size boundary, using a valid path of 5000 nodesedges = "\n".join(f"{i} {i + 1}" for i in range(1, 5000))assert run(f"1\n5000\n{edges}\n") == "Bob\n", "maximum even n"
# Multiple test cases, checking input consumptionassert run(    """4121 231 21 341 21 33 4""") == "Alice\nBob\nAlice\nBob\n", "mixed parities"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n = 1`|`Alice`| Bản thân gốc là một cây con hoàn hảo hợp pháp. | 
|`n = 2`, bờ rìa`1 2`|`Bob`| Cây chẵn nhỏ nhất và ranh giới chẵn lẻ. | 
| Cây hoàn hảo với`n = 7`|`Alice`| Cây nhị phân hoàn hảo không cần xử lý đặc biệt. | 
| Cây hợp lệ với`n = 5000`|`Bob`| Ranh giới số nút tối đa và thậm chí chẵn lẻ. | 
| Bốn trường hợp hỗn hợp |`Alice`,`Bob`,`Alice`,`Bob`| Xử lý chính xác nhiều trường hợp thử nghiệm và mức tiêu thụ cạnh. | 

## Vỏ cạnh 

Đối với cây một nút,```
11
```không có dòng cạnh để đọc. Bản thân nút duy nhất là một cây nhị phân đầy đủ hoàn hảo nên Alice có thể loại bỏ nó ngay lập tức. Thuật toán thấy n=1, là số lẻ và in ra`Alice`. 

Đối với cây chẵn nhỏ nhất,```
121 2
```động thái hợp pháp duy nhất là loại bỏ một chiếc lá. Một nút vẫn còn và người chơi khác sẽ loại bỏ nó. Trò chơi có hai nước đi nên Bob thắng. Thuật toán in`Bob`vì 2 là số chẵn. 

Một cái cây hoàn hảo có thể trông có vẻ đặc biệt nhưng nó tuân theo cùng một quy tắc. Vì```
171 21 32 42 53 63 7
```Alice có thể loại bỏ toàn bộ cây bảy nút chỉ bằng một nước đi, mang lại chiến thắng ngay lập tức. Tổng quát hơn, một cây hoàn hảo có 2 nút h+1 −1, luôn là số lẻ, do đó câu trả lời của nó là tự động`Alice`. 

Cuối cùng, hình dạng có thể cực kỳ mất cân đối. Một đường dẫn vẫn là cây nhị phân hợp lệ vì mỗi nút có nhiều nhất hai nút con. Đối với một đường dẫn chứa 5000 nút, hầu hết các cây con đều không hoàn hảo, nhưng mỗi lá vẫn là một nước đi hợp pháp. Các động thái chính xác có sẵn không quan trọng. Vì 5000 là số chẵn nên mỗi lượt chơi hoàn chỉnh đều có số nước đi chẵn và Bob thắng. Đây là lý do tại sao việc triển khai có thể bỏ qua tất cả các điểm cuối biên một cách an toàn sau khi sử dụng chúng.
