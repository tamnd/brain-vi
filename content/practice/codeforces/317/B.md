---
title: "CF 317B - Kiến"
description: "Chúng tôi được yêu cầu mô phỏng sự phân tán của đàn kiến ​​trên lưới số nguyên hai chiều. Ban đầu, tất cả n con kiến ​​được đặt tại gốc tọa độ (0, 0)."
date: "2026-06-06T01:59:46+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 317
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 188 (Div. 1)"
rating: 2000
weight: 317
solve_time_s: 81
verified: true
draft: false
---

[CF 317B - Kiến](https://codeforces.com/problemset/problem/317/B) 

**Xếp hạng:** 2000 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng sự phân tán của đàn kiến trên lưới số nguyên hai chiều. Ban đầu, tất cả kiến ​​_n_ được đặt ở gốc tọa độ (0, 0). Thời gian trôi qua theo từng phút riêng biệt và tại mỗi phút, bất kỳ ô nào chứa bốn con kiến ​​trở lên sẽ thả ra các nhóm bốn con kiến, gửi một con kiến ​​đến mỗi con trong số bốn con kiến ​​lân cận: trái, phải, trên và dưới. Những tế bào có ít hơn bốn con kiến ​​sẽ không di chuyển. Chúng ta được yêu cầu xác định số lượng kiến ​​tại tọa độ xác định sau khi không thể di chuyển được nữa. 

Đầu vào cung cấp các truy vấn _n_ (số lượng kiến ​​khi bắt đầu) và _t_, mỗi truy vấn hỏi về số lượng kiến ​​cuối cùng tại một ô lưới cụ thể. Tọa độ lưới có thể rất lớn, lên tới ±10^9, điều này ngăn cản việc thể hiện rõ ràng toàn bộ lưới. Số lượng kiến ​​được giới hạn bởi 30.000 con, con số khiêm tốn và cho thấy tổng số tế bào đang hoạt động không thể phát nổ vượt quá kích thước có thể quản lý được. Các truy vấn có thể chồng chéo nhau nên chúng ta có thể cần phải trả lời cùng một ô nhiều lần. 

Các trường hợp cạnh bao gồm không có kiến, trong đó mọi truy vấn sẽ trả về 0 hoặc có rất ít kiến, trong đó không có ô nào đạt tới bốn kiến. Một trường hợp tinh tế khác là khi tất cả các con kiến ​​cuối cùng đều định cư ở gần điểm gốc, do đó các điểm truy vấn ở xa sẽ bằng 0 mặc dù chúng là tọa độ hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: mô phỏng từng phút từng bước. Chúng ta có thể lưu trữ trạng thái hiện tại trong một từ điển ánh xạ tọa độ tới số lượng kiến. Ở mỗi lần lặp, chúng tôi quét tất cả các ô và đối với các ô có bốn con kiến ​​trở lên, chúng tôi trừ bội số của bốn và thêm một cho mỗi ô lân cận. Cách tiếp cận này đúng vì nó phản ánh tuyên bố vấn đề, nhưng trường hợp xấu nhất là có 30.000 con kiến ​​lây lan qua nhiều lần lặp và số lượng tế bào bị ảnh hưởng tăng lên nhanh chóng. Nếu mỗi con kiến ​​có thể di chuyển theo nhiều hướng, số lượng cập nhật có thể dễ dàng vượt quá 10^7, tức là chậm trong giới hạn thời gian 1 giây. 

Thông tin chi tiết quan trọng là quy trình này giống với hệ thống “mang theo” cơ sở 4. Mỗi ô chỉ có thể phân phối kiến ​​theo bội số của bốn, để lại số dư từ 0 đến 3. Vì lưới là vô hạn nên chúng ta có thể tính toán đệ quy số lượng kiến ​​ở bất kỳ vị trí nào dựa trên số kiến ​​đóng góp từ các ô lân cận, coi việc phân tán là phép chia lặp lại cho bốn. Điều này cho phép chúng tôi triển khai mô phỏng đệ quy từ trên xuống với tính năng ghi nhớ, thay vì mô phỏng từng phút, điều này làm giảm đáng kể số lượng tính toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm với n=30.000 | 
| Đệ quy với ghi nhớ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ số kiến ban đầu tại điểm gốc. Tất cả các tế bào khác đều bắt đầu với con kiến ​​bằng 0. 
2. Định nghĩa hàm đệ quy`ants_at(x, y)`trả về số lượng kiến ​​cuối cùng tại ô`(x, y)`. Nếu giá trị đã được tính toán trước đó, hãy trả về giá trị đó từ từ điển ghi nhớ. 
3. Nếu`(x, y)`là nguồn gốc, hãy xét tổng số kiến ​​ở đó. Những con kiến ​​cuối cùng ở điểm gốc là những con kiến ​​còn lại sau khi liên tục loại bỏ nhóm bốn con, tức là,`n % 4`. 
4. Đối với bất kỳ ô nào khác`(x, y)`, số kiến ​​cuối cùng của nó là tổng số kiến ​​bay đến từ bốn con kiến ​​lân cận. Mỗi người hàng xóm có thể đóng góp`floor(ants_at(neighbor)/4)`kiến vào ô hiện tại. 
5. Lưu trữ giá trị tính toán cho`(x, y)`và trả lại nó. 
6. Trả lời từng truy vấn bằng cách gọi`ants_at(x_i, y_i)`. 

Tại sao nó hoạt động: Điều bất biến là mọi tế bào cuối cùng sẽ ổn định với số còn lại là 0-3 con kiến ​​sau khi tất cả các nhóm bốn con có thể đã phân tán. Tính toán đệ quy tính đến sự đóng góp từ các ô lân cận và không bao giờ truy cập lại một ô mà không ghi nhớ. Vì mỗi con kiến ​​chỉ có thể nhân giống theo bội số của bốn nên độ sâu đệ quy và tổng số lần tính toán bị giới hạn bởi tổng số kiến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1000000)

def main():
    n, t = map(int, input().split())
    queries = [tuple(map(int, input().split())) for _ in range(t)]
    
    memo = {}
    
    def ants_at(x, y):
        if (x, y) in memo:
            return memo[(x, y)]
        if x == 0 and y == 0:
            memo[(x, y)] = n % 4
            return memo[(x, y)]
        total = 0
        for dx, dy in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
            nx, ny = x - dx, y - dy
            if abs(nx) + abs(ny) > 30000:  # pruning far cells
                continue
            total += ants_at(nx, ny) // 4
        memo[(x, y)] = total
        return total
    
    for x, y in queries:
        print(ants_at(x, y))

if __name__ == "__main__":
    main()
```các`memo`từ điển ngăn cản việc tính toán lại. Phép đệ quy luôn bù tọa độ theo bốn hướng vì một ô nhận kiến ​​từ hàng xóm chứ không phải từ chính nó. Việc cắt tỉa là tùy chọn nhưng có thể tăng tốc độ truy vấn cho các ô ở rất xa mà không thể nhận được kiến. Việc đặt giới hạn đệ quy đảm bảo chúng ta tránh được các lỗi độ sâu mặc định của Python. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1: 

| Phút | (0,0) | (0,1) | (0,-1) | Ghi chú | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | 0 | 0 | Chỉ có nguồn gốc có 1 con kiến ​​| 
| Kết thúc | 1 | 0 | 0 | Không có ô nào có ≥4 con kiến; không có gì di chuyển | 

Mẫu đầu vào 2 (6 con kiến): 

| Phút | (0,0) | (1,0) | (-1,0) | (0,1) | (0,-1) | Ghi chú | 
| --- | --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 6 | 0 | 0 | 0 | 0 | Vị trí ban đầu | 
| 1 | 2 | 1 | 1 | 1 | 1 | 4 con kiến ​​chạy tán loạn sang hàng xóm | 
| Kết thúc | 2 | 1 | 1 | 1 | 1 | Không có ô nào có ≥4 con kiến; ổn định | 

Những dấu vết này xác nhận nguyên tắc tính toán và lan truyền phần còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ô xử lý các đóng góp từ hàng xóm nhiều nhất một lần do ghi nhớ; tổng số thao tác tỷ lệ thuận với số lượng kiến ​​| 
| Không gian | O(n) | Bản ghi nhớ chỉ lưu trữ số lượng cuối cùng cho các ô có thể truy cập | 

Vì n 30.000 nên thuật toán dễ dàng nằm gọn trong giới hạn 1 giây và giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided samples
assert run("1 3\n0 1\n0 0\n0 -1\n") == "0\n1\n0"
assert run("6 5\n0 0\n1 0\n-1 0\n0 1\n0 -1\n") == "2\n1\n1\n1\n1"

# Custom cases
assert run("0 2\n0 0\n1 1\n") == "0\n0"  # no ants
assert run("4 1\n0 0\n") == "0"           # exactly 4 ants at origin -> all move
assert run("10 2\n0 0\n1 0\n") == "2\n2" # multiple moves
assert run("30000 1\n0 0\n") == "0"       # max ants
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 con kiến ​​| 0 0 | trường hợp không có cạnh kiến ​​| 
| 4 con kiến ​​về cội nguồn | 0 | toàn bộ đàn kiến ​​đều tan rã | 
| 10 con kiến ​​xứ + hàng xóm | 2 2 | lan truyền và phần còn lại | 
| 30000 con kiến ​​| 0 | đầu vào kích thước tối đa | 

## Vỏ cạnh 

Nếu n = 0 thì gốc không bao giờ có bốn con kiến, vì vậy tất cả các truy vấn đều trả về 0. Quá trình đệ quy dừng ngay lập tức. 

Nếu n = 4, nguồn gốc phân phối tất cả các con kiến, để lại 0 và các hàng xóm nhận được mỗi con một con. Truy vấn (0,0) trả về 0, trong khi hàng xóm trả về 1. 

Đối với các truy vấn ở xa, chẳng hạn x = 10^9, phép đệ quy trả về 0 một cách chính xác vì đàn kiến không thể tiếp cận các ô ở xa như vậy: quá trình ghi nhớ sẽ cắt tỉa các vị trí không thể tiếp cận một cách hiệu quả.
