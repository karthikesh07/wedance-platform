<template>
  <div class="flex">
    <div class="mr-2">
      {{ getTime(item.startDate) }}
    </div>
    <div class="mr-2">
      {{ getEventIcon(item.eventType) }}
    </div>
    <div>
      <NuxtLink
        :to="`/events/${item.id}`"
        class="font-bold leading-none hover:underline hover:text-primary"
      >
        {{ item.name }}
      </NuxtLink>
      <TReactions :item="item" class="my-1" />
      <div>
        <div class="text-xs flex flex-wrap gap-1">
          <div v-if="item.online === 'Yes'">
            <div class="flex items-center gap-1">
              <TIcon name="youtube" class="w-4 h-4" />
              <p>
                Online
              </p>
            </div>
          </div>
          <div v-if="item.venue" class="flex items-center gap-1">
            <template v-if="currentCity === item.place">
              <TIcon name="place" class="w-4 h-4" />
              <p>
                {{ item.venue.name }}
              </p>
            </template>
            <template v-else>
              <TIcon name="place" class="w-4 h-4" />
              <p>
                {{ addressPart(item.venue, 'locality') }}
              </p>
            </template>
          </div>
        </div>
        <div class="text-xs flex flex-wrap gap-1">
          <div v-if="item.price">
            <div class="flex items-center gap-1">
              <TIcon name="ticket" class="w-4 h-4" />
              <p>
                {{ item.price }}
              </p>
            </div>
          </div>
          <div v-if="item.org" class="text-primary">
            {{ item.org.username }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { getTime } from '~/utils'
import { useRsvp } from '~/use/rsvp'
import { useApp } from '~/use/app'
import { useEvents } from '~/use/events'
import { useCities } from '~/use/cities'
import { addressPart } from '~/use/google'

export default {
  name: 'TEventText',
  setup() {
    const { updateRsvp } = useRsvp()
    const { getCity } = useApp()
    const { currentCity } = useCities()
    const { getEventIcon } = useEvents()

    return {
      getTime,
      updateRsvp,
      getCity,
      currentCity,
      addressPart,
      getEventIcon,
    }
  },
  props: {
    item: {
      type: Object,
      default: () => ({}),
    },
  },
}
</script>
