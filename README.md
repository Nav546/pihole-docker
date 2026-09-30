Architecture diagram: a Mermaid diagram that GitHub renders automatically, plus a short "how a lookup flows" explanation.
Config table: all six .env variables in one place.
Windows note: why DNS is on port 5300.
Tests and screenshots: docker exec tests with your three docs/ screenshots and captions.
Fixes to the old text: the volume name is now pihole-docker_pihole_data, which is what your Compose config actually creates. The troubleshooting section no longer mentions 5353. I also removed the stray \_ characters that had crept into the old text
